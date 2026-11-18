# Relatório RAZÃO — contrato e desenho técnico

> Documento técnico (não publicado). Arquitetura geral: [ARCHITECTURE.md](ARCHITECTURE.md).
> Status: **contrato v1 para implementação** (T-10, 2026-11-17). Itens ⚠️ são propostas ainda **não confirmadas
> pelo Rodrigo**; o backend implementa a proposta e ajusta no código se a resposta for diferente.

## Objetivo

Listar os **pagamentos e recebimentos** de um período (padrão: o dia de hoje) e consolidar o resultado por
**forma de pagamento** (Entradas, Saídas, Saldo). Parecido com a MOV.FINANCEIRA
(`/movimentacaoFonte/relatorioMovimentacao`), mas **sem o resumo de saldos das fontes** (não depende do
Encerra Movimentação Financeira, não lê `MovimentacaoFonte`).

## Endpoint

`GET /relatorios/financeiro/razao` — **um só endpoint** devolve lançamentos + consolidação.

Por que não `/razao/listar` + `/razao/consolidar`: a consolidação é a soma exata das linhas listadas. Duas rotas
repetiriam a mesma consulta e poderiam divergir (filtro aplicado numa e não na outra). A MOV.FINANCEIRA e o
relatório de entrada de caixa já seguem o padrão "uma consulta → prévia, PDF e Excel a partir dos mesmos dados".

Estrutura (mesmo padrão de `relatorios/LancamentosEntradaCaixaController` + `Services/Relatorios/`):

| Camada | Arquivo (path-api) | Papel |
|---|---|---|
| Rota | `start/routes.ts`, junto de `/relatorios/financeiro/lancamentos-entrada-caixa` | `Route.get("/relatorios/financeiro/razao", "relatorios/RazaoController.index")` |
| Controller | `app/Controllers/Http/relatorios/RazaoController.ts` | valida a query com **yup**, chama o service, responde `200`/`400`/`500` |
| Service | `app/Services/Relatorios/RazaoService.ts` | consultas (3 fontes de dados) + regras puras exportadas (classificar, ordenar, consolidar) |
| Teste | `tests/unit/razao.test.cjs` | regras puras do service (padrão de `lancamentos-entrada-caixa.test.cjs`) |

Reaproveitar de `LancamentosEntradaCaixaService`: `TAREFAS_RECEBIMENTO` e `valorRecebido` (já exportados) e
`ehTaxaCartao` (exportar — hoje é `const` local). Não copiar as regras.

## Request (query string)

| Parâmetro | Tipo | Obrigatório | Padrão (front) | Regra |
|---|---|---|---|---|
| `EmpresaId` | int | sim | empresa da sessão | tenant + unidade |
| `DataI` | `yyyy-MM-dd` | sim | hoje | início do período de **LancamentoData** ⚠️1 |
| `DataF` | `yyyy-MM-dd` | sim | hoje | fim; `DataI <= DataF`; no máximo **366 dias** |
| `Baixas` | `TODAS` \| `MANUAIS` \| `BANCARIAS` | não | `TODAS` | ver "Regras" |
| `ClassificadoPor` | `LANCAMENTO` \| `RECIBO` \| `NOME` \| `MATRICULA` | não | `LANCAMENTO` ⚠️5 | ordenação dentro do dia |
| `ConsiderarAbastecimento` | `1` \| `0` | não | `0` ⚠️4 | checkbox "Considerar Abastece MeuCaixa" |
| `ConsiderarSangria` | `1` \| `0` | não | `0` ⚠️4 | checkbox "Considerar Sangria" |
| `Operador` | string (Apelido) | não | todos | `DiarioFinanceiro.LancamentoOperador` / `Usuarios.Apelido` |

**Atenção:** o middleware `UpperCase` converte os valores da query para MAIÚSCULAS (a rota não contém `report`).
Por isso os enums são em maiúsculas e sem acento. Exemplo:

```
GET /relatorios/financeiro/razao?EmpresaId=1&DataI=2026-11-17&DataF=2026-11-17&Baixas=TODAS
    &ClassificadoPor=RECIBO&ConsiderarAbastecimento=1&ConsiderarSangria=1
```

Esboço de validação:

```ts
const data = (r: string) => yup.string().required(`${r} é obrigatória.`).matches(/^\d{4}-\d{2}-\d{2}$/, `${r} inválida.`)
const flag = () => yup.string().oneOf(['0', '1', 'TRUE', 'FALSE']).nullable()
const schema = yup.object({
  EmpresaId: yup.number().integer().required('Empresa é obrigatória.'),
  DataI: data('Data inicial'),
  DataF: data('Data final'),
  Baixas: yup.string().oneOf(['TODAS', 'MANUAIS', 'BANCARIAS']).nullable(),
  ClassificadoPor: yup.string().oneOf(['LANCAMENTO', 'RECIBO', 'NOME', 'MATRICULA']).nullable(),
  ConsiderarAbastecimento: flag(),
  ConsiderarSangria: flag(),
  Operador: yup.string().nullable(),
})
// + DataI <= DataF e no máximo 366 dias → 400 { error: [...] } (mesmo formato do LancamentosEntradaCaixaController)
```

## Regras de negócio

**Fontes de dados** (data-base = dia do LANÇAMENTO, igual ao cálculo do Caixa em `utilities/calculoCaixa.ts`):

1. `DiarioFinanceiro`: `Opcao IN ('INC','EST')`, tarefas `RECMEN, RECPAG, RECADI, BAIAUT, OUTPAG, APAGAR`.
   - **Ficam de fora:** Transferência de Fonte (`TRFFON`), taxa/tarifa de cartão (`ehTaxaCartao`: flag `TaxaCartao`
     ou Referência), outras tarefas (`EMICAR`, `ARQREM`…).
   - **Não** filtra `Ano`/`Sequencial`: o caixa é por dia e unidade, como na MOV.FINANCEIRA e no Encerramento de Caixa.
2. `Abastecimento` — só com `ConsiderarAbastecimento=1`.
3. `Sangria` — só com `ConsiderarSangria=1`.

`Abastecimento` e `Sangria` **não têm `EmpresaId`**: o tenant/unidade vem de `Usuarios.EmpresaId` (join por `UsuarioId`).

**Coluna (Entrada × Saída)** — mesma regra de `somarMovimentacoes`:

| Lançamento | Coluna |
|---|---|
| Recebimento (`RECMEN/RECPAG/RECADI/BAIAUT/OUTPAG`) `INC` | E |
| Recebimento `EST` (estorno) | S |
| `APAGAR` `INC` (pagamento) | S |
| `APAGAR` `EST` (estorno de pagamento) | E |
| Abastecimento | E |
| Sangria (`ValorDinheiro` e `ValorCheque`, uma linha para cada valor > 0) | S |

**Valor:** sempre positivo, em centavos internamente. `BAIAUT` = `valorRecebido` (Valor + Multa + Juros +
Complementares 01–06); demais = `|Valor|`.

**Estornos** ⚠️3: a linha `EST` aparece no dia em que foi feita, na coluna oposta; o lançamento original continua
no dia dele. O par pagamento + estorno de conta a pagar **aparece** (é um razão: mostra tudo). Ele **não** é anulado
como em `anularPagamentosEstornados` do caixa, e o saldo do período fica igual nos dois casos.

**Filtro Baixas** (`BAIAUT` = baixa bancária, mesma convenção de `LancamentosEntradaCaixaService`) ⚠️2:

| Valor | Efeito |
|---|---|
| `TODAS` | sem restrição |
| `MANUAIS` | remove `BAIAUT` (fica só a baixa manual: `RECMEN/RECPAG/RECADI` no balcão) |
| `BANCARIAS` | remove `RECMEN/RECPAG/RECADI` (fica só `BAIAUT`) |

O filtro age **só sobre as baixas de mensalidade**: `OUTPAG`, `APAGAR`, Abastecimento e Sangria continuam
conforme os outros filtros. Proposta: `tarefasDoFiltroRazao(baixas)` exportada e testada.

**Classificado por** (o dia vem sempre primeiro, para o subtotal diário):

| Valor | Ordem |
|---|---|
| `LANCAMENTO` | `LancamentoData, LancamentoHora, Id` (ordem de lançamento) |
| `RECIBO` | `LancamentoData, NumeroRecibo (nulos por último), LancamentoHora` |
| `NOME` | `LancamentoData, Nome, LancamentoHora` |
| `MATRICULA` | `LancamentoData, Matricula (nulos por último), LancamentoHora` |

A ordenação final é feita **no service (JS)**, porque junta três consultas. Abastecimento/Sangria não têm recibo
nem matrícula: com `RECIBO`/`MATRICULA`, vão para o fim do dia.

**Consolidação por forma de pagamento** ⚠️4:

- `porFormaPagamento`: uma linha por `FormasPagamentoId` (rótulo `FormasPagamento.Descricao`). `BAIAUT` vai para a
  linha própria **"BAIXA BANCÁRIA"** (chave `BAIXA`). Abastecimento → linha **"ABASTECE MEU CAIXA"** (Tipo `1`). Sangria → linhas
  **"SANGRIA - DINHEIRO"** (Tipo `1`) e **"SANGRIA - CHEQUE"** (Tipo `2`). Lançamento sem forma → **"SEM FORMA DE PAGAMENTO"**.
- `porTipo`: a mesma soma agrupada por `FormasPagamento.Tipo` (`1` DINHEIRO, `2` CHEQUE, `3` CARTÃO, `4` OUTROS) +
  `BAIXA` (BAIXA BANCÁRIA). Aqui Abastecimento/Sangria somam em DINHEIRO/CHEQUE. Serve de quadro-resumo e de
  alternativa caso o Rodrigo prefira o agrupamento da MOV.FINANCEIRA.
- `Saldo = Entradas − Saidas` em cada linha e no total.

## Response `200`

```jsonc
{
  "filtros": {                       // eco dos filtros já normalizados (o template de PDF usa)
    "DataI": "2026-11-17", "DataF": "2026-11-17", "Baixas": "TODAS", "ClassificadoPor": "RECIBO",
    "ConsiderarAbastecimento": true, "ConsiderarSangria": true, "Operador": null
  },
  "lancamentos": [
    {
      "Chave": "DF-1418",            // único na lista: DF-<DiarioFinanceiroId> | AB-<AbastecimentoId> | SG-<SangriaId>-D|C
      "Origem": "DIARIO",            // DIARIO | ABASTECIMENTO | SANGRIA
      "DiarioFinanceiroId": 1418,    // null fora do Diário
      "LancamentoData": "2026-11-17",
      "LancamentoHora": "09:42",
      "Operador": "MARIA",
      "NumeroRecibo": 5120,          // null em Abastecimento/Sangria
      "Matricula": 2031,             // null quando não houver
      "Nome": "FULANO DE TAL",       // Abastecimento: "ABASTECE MEU CAIXA"; Sangria: "SANGRIA"
      "Referencia": "MENSALIDADE 11/2026", // Sangria: Observacao
      "Tarefa": "RECMEN",            // ABASTEC | SANGRIA nas origens não-Diário
      "Opcao": "INC",                // INC | EST (null fora do Diário)
      "Estorno": false,
      "TipoBaixa": "MANUAL",         // MANUAL (RECMEN/RECPAG/RECADI) | BANCARIA (BAIAUT) | null
      "FormasPagamentoId": 3,        // null em BAIAUT sem forma, Abastecimento e Sangria
      "FormaPagamento": "DINHEIRO",  // rótulo da linha de consolidação (ver regras)
      "FormaPagamentoTipo": "1",     // 1 DINHEIRO | 2 CHEQUE | 3 CARTÃO | 4 OUTROS | null
      "Fonte": "CAIXA",              // Fontes.Codigo (null fora do Diário)
      "MeuCaixa": true,              // DiarioFinanceiro.FlgMeuCaixa; true em Abastecimento/Sangria
      "Coluna": "E",                 // E = entrada | S = saída
      "Valor": 350.00                // sempre positivo, 2 casas
    }
  ],
  "porDia": [
    { "Data": "2026-11-17", "Quantidade": 42, "Entradas": 9800.00, "Saidas": 1250.00, "Saldo": 8550.00 }
  ],
  "consolidado": {
    "porFormaPagamento": [
      { "Chave": "FP-3", "FormaPagamento": "DINHEIRO", "Tipo": "1", "Quantidade": 20,
        "Entradas": 4200.00, "Saidas": 300.00, "Saldo": 3900.00 },
      { "Chave": "BAIXA", "FormaPagamento": "BAIXA BANCÁRIA", "Tipo": null, "Quantidade": 12,
        "Entradas": 3600.00, "Saidas": 0, "Saldo": 3600.00 },
      { "Chave": "ABASTEC", "FormaPagamento": "ABASTECE MEU CAIXA", "Tipo": "1", "Quantidade": 1,
        "Entradas": 200.00, "Saidas": 0, "Saldo": 200.00 },
      { "Chave": "SANGRIA-D", "FormaPagamento": "SANGRIA - DINHEIRO", "Tipo": "1", "Quantidade": 1,
        "Entradas": 0, "Saidas": 500.00, "Saldo": -500.00 }
    ],
    "porTipo": [
      { "Chave": "1", "Descricao": "DINHEIRO", "Quantidade": 22, "Entradas": 4400.00, "Saidas": 800.00, "Saldo": 3600.00 },
      { "Chave": "BAIXA", "Descricao": "BAIXA BANCÁRIA", "Quantidade": 12, "Entradas": 3600.00, "Saidas": 0, "Saldo": 3600.00 }
    ],
    "totais": { "Quantidade": 42, "Entradas": 9800.00, "Saidas": 1250.00, "Saldo": 8550.00 }
  }
}
```

Ordem de `porFormaPagamento`: formas do Diário por `Descricao`, depois `BAIXA`, `ABASTEC`, `SANGRIA-D`, `SANGRIA-C`.
Ordem de `porTipo`: `1, 2, 3, 4, BAIXA`. Linhas com 0 lançamentos não aparecem. Período sem nada: listas vazias e
totais zerados (`200`, não `404`); a tela mostra o estado vazio.

Erros: `400 { error: string[] }` (validação/período) · `500 { error: 'Erro ao gerar o relatório RAZÃO.' }`
(log no servidor, sem SQL/stack na resposta).

## Consultas SQL (esboço)

Todas via `dbCon` (tenant) e com bindings. As três rodam em paralelo (`Promise.all`); as duas últimas só se o checkbox estiver marcado.

```sql
-- 1) Diário Financeiro
SELECT df.DiarioFinanceiroId, DATE_FORMAT(df.LancamentoData,'%Y-%m-%d') AS LancamentoData, df.LancamentoHora,
       df.LancamentoOperador AS Operador, df.NumeroRecibo, df.Matricula, df.Nome, df.Referencia,
       df.Tarefa, df.Opcao, df.FormasPagamentoId, df.FlgMeuCaixa + 0 AS MeuCaixa, df.TaxaCartao + 0 AS TaxaCartao,
       df.Valor, df.ValorMulta, df.ValorJuros, df.ValorComplementar01, df.ValorComplementar02, df.ValorComplementar03,
       df.ValorComplementar04, df.ValorComplementar05, df.ValorComplementar06,
       fp.Descricao AS FormaPagamento, fp.Tipo AS FormaPagamentoTipo, f.Codigo AS Fonte
  FROM DiarioFinanceiro df
  LEFT JOIN FormasPagamento fp ON fp.FormasPagamentoId = df.FormasPagamentoId
  LEFT JOIN Fontes f          ON f.FonteId = df.FonteId
 WHERE df.EmpresaId = :EmpresaId
   AND df.LancamentoData BETWEEN :DataI AND :DataF
   AND df.Opcao IN ('INC','EST')
   AND df.Tarefa IN (:tarefas)                 -- tarefasDoFiltroRazao(Baixas) + 'OUTPAG','APAGAR'
   AND (df.TaxaCartao IS NULL OR df.TaxaCartao = 0)
   [AND df.LancamentoOperador = :Operador]
-- taxa pela Referência (lançamentos antigos sem a flag): filtrar no service com ehTaxaCartao

-- 2) Abastecimento (ConsiderarAbastecimento=1)
SELECT a.AbastecimentoId, DATE_FORMAT(a.AbastecimentoData,'%Y-%m-%d') AS LancamentoData,
       a.AbastecimentoHora AS LancamentoHora, u.Apelido AS Operador, a.Valor
  FROM Abastecimento a
  INNER JOIN Usuarios u ON u.UsuarioId = a.UsuarioId
 WHERE u.EmpresaId = :EmpresaId
   AND a.AbastecimentoData BETWEEN :DataI AND :DataF
   [AND u.Apelido = :Operador]

-- 3) Sangria (ConsiderarSangria=1)
SELECT s.SangriaId, DATE_FORMAT(s.LancamentoData,'%Y-%m-%d') AS LancamentoData, s.LancamentoHora,
       u.Apelido AS Operador, s.ValorDinheiro, s.ValorCheque, s.Observacao
  FROM Sangria s
  INNER JOIN Usuarios u ON u.UsuarioId = s.UsuarioId
 WHERE u.EmpresaId = :EmpresaId
   AND s.LancamentoData BETWEEN :DataI AND :DataF
   [AND u.Apelido = :Operador]
```

Datas com `DATE_FORMAT` no SELECT para não mudar o dia (o typeCast de `DATE` cria `Date` em UTC — ver
`EncerramentoCaixaController.soData`).

**Índices:** `DiarioFinanceiro` só tem PK + `TaxaCartao`. Para períodos longos, recomenda-se
`(EmpresaId, LancamentoData)`. Também ajuda o Encerramento de Caixa e o relatório de entrada de caixa. É **DDL**:
script idempotente em `database/scripts/` e aplicação em todos os bancos **só com confirmação do Rodrigo**. Não
bloqueia a v1.

## Frontend (harpia-ui) — referência para a tarefa de tela

- Página `src/pages/Impressao/Relatorio_Razao.tsx`, rota `/Relatorios/Financeiro/Razao` (lazy em `config/Routes.tsx`),
  template de PDF `src/templates/razao.ts`. Modelo: `Relatorio_MovimentacaoFinanceira.tsx`, sem o bloco de
  "Resumo de saldos" e sem o aviso do Encerra Movimentação.
- Filtros: período (padrão hoje), Operador (`/usuario/listaUsuarioEmpresa`), Baixas (radio Todas/Somente
  manuais/Somente bancárias), Classificado por (radio), 2 checkboxes, Título (só na impressão).
- Visualizar → prévia (lançamentos agrupados por dia com subtotal de `porDia` + quadro de consolidação);
  Imprimir (`/gerarRelatorio`) e Excel usam os mesmos dados da prévia.
- Permissão `Imprimir` via `usePermissao()`; menu/ajuda: página no manual (`harpia-docs/docs/`) + `npm run ia:validar`.

## Regras confirmadas e dúvidas abertas

Confirmadas pelo Rodrigo em 2026-11-17 (diálogo da T-10):

- 1 **Data-base**: `LancamentoData` (dia do caixa).
- 2 **Escopo do filtro Baixas**: age só nas baixas de mensalidade; OUTPAG/APAGAR permanecem.
- 3 **Estornos**: linhas `EST` aparecem na coluna oposta, sem anular o par pagamento/estorno; TRFFON e taxa de cartão fora.
- 4 **Consolidação**: `porFormaPagamento` por Descrição + linha "BAIXA BANCÁRIA"; Abastecimento em DINHEIRO e Sangria
  em DINHEIRO/CHEQUE, só com o checkbox marcado; `porTipo` como resumo.

Ainda abertas:

- ⚠️4 **Padrão dos checkboxes**: proposto desmarcados. Não foi perguntado.
- ⚠️5 **Classificação padrão**: proposto `LANCAMENTO`. A MOV.FINANCEIRA usa `Recibo`. Não foi perguntado.
