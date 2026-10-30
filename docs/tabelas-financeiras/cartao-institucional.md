# Cartão Institucional

## Objetivo

Cadastrar os **cartões institucionais** da escola — os cartões usados para **pagar despesas**.
Ficam vinculados a um centro de custo, uma fonte sugerida e um dia de vencimento, e alimentam a
[Fatura de Cartão](../financeiro/fatura-cartao.md).

---

## Como acessar

```
Tabelas
→ Cartão Institucional
```

---

## Quando utilizar

- Ao cadastrar um cartão corporativo/da escola usado para pagamentos.

---

## Como utilizar

1. Abra **Cartão Institucional**.
2. Clique em **Novo**.
3. Informe o **Nome do Cartão**, o **Número** e o **Tipo**.
4. Defina o **Dia Padrão de Vencimento**, o **Dia de Fechamento da Fatura** (obrigatório para cartão do tipo **Crédito**), o **Centro de Custo** e a **Fonte Sugerida**.
5. Clique em **Gravar**.

![Lista de cartões com a coluna Dia de Fechamento da Fatura](../assets/images/tabelas-financeiras/cartao-institucional-lista.jpg)

![Cadastro: campo Dia de Fechamento da Fatura (habilitado só para Crédito)](../assets/images/tabelas-financeiras/cartao-institucional-novo.jpg)

---

## Campos

| Campo | Descrição | Obrigatório |
|-------|-----------|-------------|
| Nome do Cartão | Identificação do cartão | Sim |
| Número | Número/identificação | Não |
| Tipo | Tipo do cartão | Não |
| Dia Padrão de Vencimento | Vencimento da fatura | Não |
| Dia de Fechamento da Fatura | Dia do mês (1 a 31) em que a fatura fecha. Só habilitado para cartão do tipo **Crédito**; também aparece como coluna na lista | Sim, para Crédito |
| Centro de Custo | Centro de custo padrão | Não |
| Fonte Sugerida | Fonte para pagamento da fatura | Não |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Novo** | Cadastra um novo cartão institucional |
| **Pesquisar** | Busca cartões |
| **Gravar** | Salva o registro |
| **Ícones da linha (lápis / lixeira)** | Editar ou excluir o registro (a lixeira aparece só para quem tem permissão de excluir). O antigo botão **Opções** do topo foi removido |

---

## Resultados

- O cartão fica disponível para lançar despesas e gerar a [Fatura de Cartão](../financeiro/fatura-cartao.md).

---

## Regras importantes

- O **dia de vencimento** e a **fonte sugerida** agilizam a quitação da fatura.

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Novo] --> B[Nome, número, tipo]
    B --> C[Vencimento, centro de custo, fonte]
    C --> D[Gravar]
    D --> E[Usar em despesas e Fatura de Cartão]
```

---

## Exemplos práticos

**Exemplo:** cadastrar o "Cartão Corporativo — final 1234", vencimento dia 10, centro de custo Administrativo.

---

## Dicas

- Vincule o **centro de custo** certo para os relatórios de despesa saírem corretos.

---

## FAQ

**Como pago a fatura desse cartão?**
Em [Fatura de Cartão](../financeiro/fatura-cartao.md).

---

## Prints

- [ ] Lista de cartões institucionais.
- [ ] Formulário.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. |
| 30/10/2026 | Novo campo **Dia de Fechamento da Fatura** (obrigatório para cartão de crédito; aviso *"Informe o dia de fechamento da fatura entre 1 e 31."*). Usado na [Fatura de Cartão](../financeiro/fatura-cartao.md). |
