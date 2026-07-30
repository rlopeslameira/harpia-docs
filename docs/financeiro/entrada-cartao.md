# Entrada de Cartão

## Objetivo

Confirmar a **entrada (crédito) dos pagamentos em cartão** — quando a operadora efetiva o
crédito dos valores recebidos em cartão, registrando a data de crédito e a tarifa cobrada. É a
conciliação das vendas em cartão com o que a operadora deposita.

---

## Quem pode utilizar

Financeiro e Administrador.

---

## Como acessar

```
Financeiro
→ Entrada de Cartão
```

---

## Quando utilizar

- Quando a operadora credita os valores das transações de cartão.

---

## Como utilizar

1. Abra **Entrada de Cartão**.
2. Filtre pelos períodos (**Data Lançamento**, **Data Disponível**) e pela **Forma de Pagamento** (cartão).
3. Clique em **Pesquisar** para listar as transações pendentes de crédito.
4. Informe a **Data do Crédito** e o **Valor Tarifa** de cada transação.
5. Clique em **Confirmar Entrada de Cartão**.

---

## Campos

| Campo | Descrição | Obrigatório |
|-------|-----------|-------------|
| Forma de Pagamento | Cartão a conciliar | Sim |
| Data Lançamento (Inicial/Final) | Período de lançamento | Não |
| Data Disponível (Inicial/Final) | Período de disponibilidade | Não |
| Data do Crédito | Data em que a operadora creditou | Sim |
| Valor Tarifa | Tarifa cobrada pela operadora | Não |
| Nº Documento | Documento da transação | Não |
| Matrícula / Nome | Aluno da transação | — |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Pesquisar** | Lista as transações de cartão |
| **Confirmar Entrada de Cartão** | Registra o crédito e a tarifa |

---

## Resultados

- As transações ficam **creditadas**, com a tarifa registrada, conciliando o cartão.

---

## Regras importantes

- Informe a **Data do Crédito** para confirmar a entrada.
- A **tarifa** reduz o valor líquido creditado.

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Filtrar transações de cartão] --> B[Pesquisar]
    B --> C[Informar data de crédito e tarifa]
    C --> D[Confirmar Entrada de Cartão]
    D --> E[Cartão conciliado]
```

---

## Exemplos práticos

**Exemplo:** confirmar o crédito das vendas de cartão da semana, informando a data em que caíram
na conta e a tarifa da operadora.

---

## Dicas

- Concilie regularmente para o saldo bancário refletir o líquido do cartão.

---

## FAQ

**Onde recebo o pagamento em cartão inicialmente?**
No [Recebimento de Mensalidade](recebimento-mensalidade.md)/[Outros Recebimentos](outros-recebimentos.md);
aqui você confirma o **crédito** posterior.

---

## Prints

- [ ] Filtros e lista de transações.
- [ ] Confirmação de entrada.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. Ajustes nas telas de Cartão/Entrada de Cartão (jul/2026). |
