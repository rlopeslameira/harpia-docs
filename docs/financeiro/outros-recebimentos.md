# Outros Recebimentos

## Objetivo

Registrar recebimentos que **não são mensalidade** — por exemplo taxas, materiais, uniformes,
segunda via de documentos, eventos e demais cobranças avulsas. A tela permite lançar o valor,
escolher a forma de pagamento e emitir o **[recibo](recibo.md)** correspondente.

---

## Como acessar

```
Financeiro
→ Outros Recebimentos
```

---

## Quando utilizar

- Cobrar valores avulsos ligados (ou não) a um aluno.
- Registrar entradas que precisam de recibo mas não estão vinculadas à mensalidade.

---

## Como utilizar

1. Abra **Outros Recebimentos**.
2. Para um novo lançamento, clique em **Novo**.
3. **Pesquise o aluno** (quando o recebimento for vinculado a um aluno).
4. Informe o **Referente** (descrição do que está sendo cobrado).
5. Escolha o **Plano de Pagamento** / **Conta** e o número de **Qtd. Parcelas**, se aplicável.
6. Informe **Valor**, e **Juros**, **Multa** e **Desconto** quando houver.
7. Escolha a **Forma de Pagamento**, a **Fonte** e a **Data**.
8. Confira o **Total** e o **Valor Pago**.
9. Clique em **Gravar**.
10. Use **Visualizar Recibo** / **Imprimir** para emitir o comprovante.

*(Print sugerido: formulário de Outros Recebimentos preenchido.)*

---

## Campos

| Campo | Descrição | Obrigatório | Exemplo | Observações |
|-------|-----------|-------------|---------|-------------|
| Aluno / Nome | Aluno vinculado ao recebimento | Condicional | Maria Souza | Pesquisa padronizada por aluno |
| Referente | Descrição do que está sendo cobrado | Sim | Taxa de 2ª via | |
| Plano de Pagamento | Plano/condição usada na cobrança | Condicional | Avulso | |
| Conta | Conta/plano de contas do recebimento | Sim | Receitas — Taxas | Mensagem de erro se vazio |
| Qtd. Parcelas | Número de parcelas do cartão | Condicional | 3 | Habilitado apenas para **cartão parcelado**. Mínimo 1 e no máximo o limite de parcelas do cartão selecionado |
| Valor | Valor da cobrança | Sim | 30,00 | Mensagem de erro se vazio |
| Juros | Juros aplicados | Não | 0,00 | |
| Multa | Multa aplicada | Não | 0,00 | |
| Desconto | Desconto concedido | Não | 0,00 | |
| Forma de Pagamento | Como o valor foi pago | Sim | Dinheiro | |
| Fonte | Origem/destino do valor | Sim | Caixa | |
| Data | Data do recebimento | Sim | 30/07/2026 | Não pode ser futura |
| Nº Documento | Nº do comprovante | Não | 4567 | |
| Bom Para / Disponível Em | Disponibilidade (cheque/cartão) | Não | 05/08/2026 | |
| Nº Recibo | Número do recibo gerado | — | 000912 | Gerado pelo sistema |
| Pagamento sem valor recebido | Marca como cortesia | Não | — | |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Novo** | Inicia um novo recebimento |
| **Pesquisar** | Busca recebimentos já lançados |
| **Gravar** | Registra o recebimento e gera o recibo |
| **Visualizar Recibo** | Mostra o recibo na tela |
| **Imprimir** | Imprime o recibo |
| **Opções** | Abre ações do registro (visualizar recibo, estornar, excluir) |
| **Configurar Impressão do Recibo** | Ajusta o layout/opções do recibo |
| **Cancelar** | Fecha sem gravar |

---

## Filtros (tela de consulta)

| Filtro | O que faz |
|--------|-----------|
| Nome | Filtra pelo nome do aluno/recebedor |
| Situação | Filtra pela situação do recebimento |
| Pesquisar | Aplica os filtros e lista os resultados |

Colunas da lista: **Seq**, **Nº Recibo**, **Nome**, **Referente**, **Parc.**, **Valor**,
**Valor Pago**, **Vencimento**, **Data Disp.**, **Data Pagto**, **Ações**.

> A coluna **Valor** mostra o valor lançado (a receber) e a coluna **Valor Pago** mostra o que
> foi efetivamente recebido — úteis para identificar rapidamente pagamentos parciais ou pendentes.

---

## Resultados

- O recebimento é registrado e lançado no **Diário Financeiro**.
- É gerado um **[recibo](recibo.md) numerado** (campo **Nº Recibo**).

---

## Regras importantes

- **Conta** e **Valor** são obrigatórios.
- **Qtd. Parcelas** só é habilitado para **cartão parcelado** que **não** seja **RPU**; deve ser no
  mínimo 1 e não pode ultrapassar o limite de parcelas do cartão selecionado. Para dinheiro/PIX,
  para cartões não parcelados e para cartões **RPU**, fica fixo em 1. Os cartões RPU aparecem no
  dropdown com a marcação **- RPU** ao final da descrição. Ver
  [Cartão](../tabelas-financeiras/cartao.md).
- **A Data de Pagamento não pode ser futura.**
- **Data de pagamento retroativa:** quando a escola ativa o controle de lançamento retroativo, a
  data informada precisa estar dentro do **limite de dias** configurado. Fora do limite, o
  lançamento é bloqueado. Ver [Configurações do Financeiro](../configuracoes/financeiro.md).
- Se o **caixa estiver encerrado** para a data, o lançamento é bloqueado.
- Registros com pagamento não podem ser excluídos — é preciso **estornar**.
- No **estorno**, o **número do recibo** do lançamento original continua sendo exibido.

---

## Mensagens do sistema

**Sucesso**
- *"Sucesso"* ao gravar; *"Estornado"* ao estornar; *"Excluído"* ao excluir.

**Erro**
- *"Conta é obrigatório"*
- *"Valor é obrigatório"*
- *"O número de parcelas deve ser no mínimo 1."* (cartão parcelado)
- *"Quantidade máxima de parcelas permitida para este cartão é X"* (ao exceder o limite do cartão)
- *"Data de Pagamento não pode ser futura."*
- *"Data de Pagamento inválida! Data de pagamento excede o limite de dias para pagamentos
  retroativos"*
- *"Erro ao gerar recibo!"* / *"Recibo não encontrado!"* / *"Erro ao buscar dados do recibo"*
- *"O Caixa está encerrado para a data: ..."*

**Confirmação**
- Confirmação de **estorno** e de **exclusão** antes de concluir a ação.

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Novo] --> B[Pesquisar aluno / informar referente]
    B --> C[Escolher conta e valor]
    C --> D[Forma de pagamento, fonte e data]
    D --> E[Gravar]
    E --> F[Lançar no Diário Financeiro]
    F --> G[Gerar Nº do recibo]
    G --> H[Visualizar / Imprimir recibo]
```

---

## Exemplos práticos

**Exemplo — taxa de segunda via:** A escola cobra R$ 30,00 pela 2ª via de um documento.
Clique em **Novo**, pesquise o aluno, **Referente** = "Taxa de 2ª via", **Conta** = Receitas—Taxas,
**Valor** = 30,00, **Forma** = Dinheiro, **Gravar** e **Imprimir**.

---

## Dicas

- Use um **Referente** claro: ele aparece na lista e no recibo.
- Antes de excluir, verifique se não é caso de **estorno**.

---

## Erros comuns

- **"Conta é obrigatório" / "Valor é obrigatório"** → preencha esses campos antes de gravar.
- **"Digite um valor válido para Parcelas..."** → informe uma quantidade de parcelas maior que 1.

---

## FAQ

**Preciso vincular a um aluno?**
Nem sempre. Recebimentos que não são de um aluno específico podem ser lançados conforme a
política da escola; quando forem de um aluno, pesquise-o para vincular o recibo.

**Como reimprimo um recibo antigo?**
Pesquise o recebimento na lista e use **Visualizar Recibo** / **Opções**. Ver
[Recibo de Recebimento](recibo.md).

---

## Prints

- [ ] Lista de Outros Recebimentos com filtros.
- [ ] Formulário preenchido.
- [ ] Recibo gerado.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. Recibo padronizado, numeração de recibo e input de pesquisa de aluno padronizado. |
| 31/07/2026 | **Qtd. Parcelas** vinculada ao limite do cartão (mín. 1; desabilitado para dinheiro/PIX). Recibo passa a exibir **forma de pagamento com nº de parcelas (Nx)** e **N° Doc.**. |
| 07/08/2026 | Nova coluna **Valor Pago** na lista (ao lado de Valor). Recibo passa a exibir a linha **Total** e a **máquina/tipo do cartão** na Forma de Pagto. |
| 21/08/2026 | Passou a valer o **limite de dias para pagamento retroativo** configurado nas Configurações do Financeiro. Cartões marcados como **RPU** aparecem com a marcação **- RPU** e não permitem parcelamento. No **estorno**, o **número do recibo** voltou a ser exibido, e o estorno de lançamentos parcelados reverte todos os lançamentos da operação. |
