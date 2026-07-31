# Ficha Financeira

## Objetivo

Consultar, em um só lugar, **toda a situação financeira de um aluno**: parcelas, vencimentos,
valores de mensalidade, acréscimos, deduções, descontos, valor corrigido, o que já foi pago e o
que está em aberto. Também permite ações sobre as parcelas e a impressão da ficha.

---

## Como acessar

```
Financeiro
→ Ficha Financeira
```

---

## Quando utilizar

- Para conferir a situação de um aluno antes de negociar ou receber.
- Para verificar parcelas em aberto, valores corrigidos e histórico de pagamentos.
- Para gerar a data de vencimento de um saldo em negociação.

---

## Como utilizar

1. Abra **Ficha Financeira**.
2. **Pesquise o aluno**.
3. Selecione a **Turma** e o **Plano de Pagamento**, se houver mais de um.
4. Confira a tabela de parcelas: **Parcela**, **Vencto**, **Mensal.**, **Desc.**, **Dedução**,
   **Acrésc.**, **Corrigido**, **Total**, **Pago**, **Data**, **Sit.**.
5. Use **Atualizar** para recalcular os valores após alguma alteração.
6. Se estiver negociando um saldo, informe a **Data de Vencimento do Saldo**.
7. Na coluna de **ações** de cada parcela, é possível **pagar**, **imprimir o recibo** de um
   pagamento e **estornar** um pagamento (ver seção **Ações por parcela**).
8. Use **Imprimir** para emitir a ficha.

*(Print sugerido: ficha financeira do aluno com a tabela de parcelas.)*

---

## Ações por parcela

Cada linha da tabela de parcelas traz botões próprios, que aparecem conforme a situação:

| Botão | O que faz |
|-------|-----------|
| **Pagar mensalidade** (ícone $) | Abre o recebimento da parcela diretamente pela ficha. Fica indisponível para parcela de aluno remanejado |
| **Imprimir recibo** (ícone impressora) | Reimprime o recibo de um pagamento já realizado. Um botão por pagamento existente na parcela |
| **Estornar pagamento** (ícone lixeira) | Estorna um pagamento realizado naquela parcela |

Ao expandir a parcela, o **detalhe** exibe os **dados do banco** e a composição dos valores
do pagamento.

---

## Campos e colunas

| Coluna | Descrição |
|--------|-----------|
| Parcela | Número da parcela |
| Vencto | Data de vencimento |
| Mensal. | Valor da mensalidade |
| Desc. | Desconto |
| Dedução | Valor deduzido |
| Acrésc. | Acréscimos |
| Corrigido | Valor corrigido (juros/multa/correção) |
| Total | Total da parcela |
| Pago | Valor já pago |
| Data | Data do pagamento |
| Sit. | Situação da parcela (em aberto, paga, etc.) |

| Campo | Descrição | Obrigatório | Observações |
|-------|-----------|-------------|-------------|
| Aluno | Aluno consultado | Sim | Pesquisa por nome/matrícula |
| Turma | Turma do aluno | Condicional | Quando há mais de uma |
| Plano de Pagamento | Plano vinculado | Condicional | Quando há mais de um |
| Situação | Situação a considerar | Não | Filtro |
| Data de Vencimento do Saldo | Vencimento do saldo em negociação | Condicional | Obrigatório ao gerar saldo |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Atualizar** | Recalcula os valores da ficha |
| **Imprimir** | Emite a ficha financeira |
| **Gravar** | Salva alterações realizadas na ficha |
| **Cancelar** | Fecha sem salvar |

---

## Resultados

- Visão consolidada da situação financeira do aluno.
- Ficha impressa para atendimento/negociação.

---

## Regras importantes

- Para gerar/negociar um saldo, **é preciso informar a Data de Vencimento do Saldo**.
- Os valores exibidos (corrigido, total) são **calculados pelo sistema**; use **Atualizar** após mudanças.

---

## Mensagens do sistema

**Sucesso**
- *"Estorno realizado com sucesso!"* (ao estornar um pagamento pela ficha)

**Atenção / Erro**
- *"Informe a data de vencimento do saldo para continuar."*
- *"Erro ao carregar os dados"* / *"Erro na consulta"*

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Pesquisar aluno] --> B[Selecionar turma e plano]
    B --> C[Visualizar parcelas e situação]
    C --> D{Negociar saldo?}
    D -->|Sim| E[Informar data de vencimento do saldo]
    D -->|Não| F[Atualizar / Imprimir]
    E --> F[Atualizar / Imprimir]
```

---

## Exemplos práticos

**Exemplo — conferência antes do recebimento:** Antes de receber, abra a Ficha Financeira do
aluno para ver quais parcelas estão em aberto e o valor corrigido de cada uma.

---

## Dicas

- Sempre clique em **Atualizar** depois de qualquer alteração para ver os valores corretos.

---

## Erros comuns

- **"Informe a data de vencimento do saldo..."** → preencha a data antes de continuar a negociação.

---

## FAQ

**A ficha mostra o que já foi pago?**
Sim, nas colunas **Pago** e **Data**, junto da situação de cada parcela.

---

## Prints

- [ ] Ficha financeira com tabela de parcelas.
- [ ] Coluna de **ações por parcela** (pagar / imprimir recibo / estornar).
- [ ] Detalhe da parcela com **dados do banco**.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. Inclui recálculos da ficha e data de vencimento do saldo. |
| 31/07/2026 | **Ações por parcela** (pagar, imprimir recibo e estornar) na própria tabela, com tooltips. Detalhe da parcela passa a exibir **dados do banco**. Ajustes de layout das colunas. |
