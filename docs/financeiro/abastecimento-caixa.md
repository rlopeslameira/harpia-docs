# Abastecimento de Caixa

## Objetivo

Registrar o **abastecimento (fundo de troco) do caixa** — o valor colocado no caixa de um usuário
para iniciar as operações do dia. É o ponto de partida do movimento de caixa.

---

## Quem pode utilizar

Financeiro e Administrador.

---

## Como acessar

```
Financeiro
→ Abastecimento de Caixa
```

---

## Quando utilizar

- No início do expediente, para lançar o fundo de troco do operador.

---

## Como utilizar

1. Abra **Abastecimento de Caixa**.
2. Clique em **Novo**.
3. Selecione o **Usuário** (operador do caixa).
4. Informe a **Data**, a **Hora** e o **Valor (R$)**.
5. Clique em **Gravar**.

---

## Campos

| Campo | Descrição | Obrigatório |
|-------|-----------|-------------|
| Usuário | Operador que recebe o abastecimento | Sim |
| Data | Data do abastecimento | Sim |
| Hora | Hora | Não |
| Valor (R$) | Valor abastecido | Sim |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Novo** | Novo abastecimento |
| **Pesquisar** | Busca abastecimentos por período/usuário |
| **Gravar** | Salva o abastecimento |
| **Opções** | Editar/excluir |
| **Confirmar/Cancelar Exclusão** | Confirma ou cancela a exclusão |

---

## Filtros e colunas

Filtros: **Data Início**, **Data Final**, **Usuário**. Colunas: **Data**, **Hora**, **Valor**,
**Abastecido Por**, **Abastecido Em**.

---

## Resultados

- O caixa do usuário passa a ter o valor de abertura, compondo o saldo do dia.

---

## Regras importantes

- **Usuário**, **Data** e **Valor** são obrigatórios.
- O abastecimento compõe o saldo do caixa considerado na [Sangria](sangria.md) e no
  [Encerramento de Caixa](encerramento-caixa.md).

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Novo] --> B[Selecionar usuário]
    B --> C[Informar data, hora e valor]
    C --> D[Gravar]
    D --> E[Fundo de troco no caixa]
```

---

## Exemplos práticos

**Exemplo:** abastecer o caixa da recepção com R$ 200,00 no início do dia.

---

## Dicas

- Registre o abastecimento **antes** de iniciar recebimentos, para o saldo do dia ficar correto.

---

## FAQ

**Isso é uma receita?**
Não; é o fundo de troco. Não representa entrada de receita, apenas saldo inicial do caixa.

---

## Prints

- [ ] Lista de abastecimentos.
- [ ] Formulário de abastecimento.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. |
