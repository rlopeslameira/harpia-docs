# Transferência de Fonte

## Objetivo

Transferir valores de uma **fonte financeira** (caixa, banco, conta) para outra — por exemplo,
mover o dinheiro do caixa para uma conta bancária. Registra origem, destino, valor e data,
mantendo os saldos das fontes corretos.

---

## Como acessar

```
Financeiro
→ Transferência de Fonte
```

---

## Quando utilizar

- Ao depositar o caixa no banco ou mover valores entre contas/fontes.

---

## Como utilizar

1. Abra **Transferência de Fonte**.
2. Selecione a **Fonte de Origem** e a **Fonte de Destino**.
3. Confira o **Saldo (R$)** da origem e informe o **Valor (R$)** a transferir.
4. Confira a **Data** da transferência (já vem com hoje). Ao escolher a data, o sistema avisa se o caixa daquele dia está encerrado.
5. Preencha **Referente** e, para contas bancárias, **Agência/Conta**.
6. Clique em **Executar**.

---

## Campos

| Campo | Descrição | Obrigatório | Observações |
|-------|-----------|-------------|-------------|
| Fonte de Origem | De onde sai o valor | Sim | |
| Fonte de Destino | Para onde vai o valor | Sim | |
| Saldo R$ | Saldo da origem | — | Exibido pelo sistema |
| Valor R$ | Valor transferido | Sim | Não pode exceder o saldo |
| Data | Data da transferência | Sim | Padrão: hoje. Não existe mais o campo *Data do Crédito* |
| Referente | Descrição | Não | |
| Agência / Conta | Dados bancários | Condicional | Para fontes bancárias |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Executar** | Efetiva a transferência entre as fontes |

---

## Resultados

- O saldo da origem diminui e o do destino aumenta **na data da transferência**.
- Débito e crédito são lançados juntos no Diário Financeiro (ou nenhum dos dois).

---

## Regras importantes

- O **caixa da data escolhida não pode estar encerrado**. Ao selecionar a **Data**, aparece um aviso fixo:
  - Caixa encerrado: *"O caixa referente à Data selecionada já está encerrado. Verifique o Encerramento de Caixa antes de continuar."* — o botão **Executar** fica desabilitado.
  - Dia sem caixa: *"Não existe caixa para esse dia. A transferência será lançada no caixa de hoje."* — não bloqueia.
- Além da data escolhida, o **caixa de hoje também não pode estar encerrado** (*"O caixa de hoje (dd/MM/aaaa) está encerrado! Não é permitido lançar valores."*), pois o lançamento entra no caixa do dia em que é feito.
- A transferência **não conta** no Total Geral do caixa.
- O valor não pode exceder o **saldo** da origem.

---

## Mensagens do sistema

- *"Caixa Encerrado para a Data de Transferência!"*
- *"Valor transferido não pode ser maior que o valor do saldo de origem!"*
- *"Erro ao salvar o registro!"*
- *"Transferência gravada no Diário Financeiro, mas o saldo das fontes não foi atualizado. Execute o Encerramento da Movimentação para esta data."*
- *"Valor Inválido!"* / *"Data Inválida!"*
- *"Registro gravado com sucesso!"*

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Selecionar origem e destino] --> B[Informar valor e data]
    B --> C{Caixa aberto e valor válido?}
    C -->|Não| D[Sistema bloqueia]
    C -->|Sim| E[Executar]
    E --> F[Saldos atualizados]
```

---

## Exemplos práticos

**Exemplo:** transferir R$ 3.000,00 do "Caixa" para a fonte "Banco X", na data de hoje.

---

## Dicas

- Confira a **Data** antes de executar: é ela que define o caixa verificado e o dia do saldo das fontes.

---

## Erros comuns

- **"Caixa Encerrado para a Data..."** → use uma data com caixa aberto ou reabra o período.

---

## FAQ

**Transferência gera receita/despesa?**
Não; é apenas movimentação entre fontes, sem afetar receita/despesa.

---

## Prints

- [ ] Seleção de origem/destino.
- [ ] Confirmação da transferência.
- [ ] Aviso amarelo de caixa encerrado na data escolhida.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. |
| 07/10/2026 | Campo **Data do Crédito** removido; aviso por data (caixa encerrado/inexistente), bloqueio do caixa de hoje e novas mensagens. |
