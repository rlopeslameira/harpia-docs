# Forma de Ingresso

## Objetivo

Cadastrar as **formas de ingresso** do aluno na escola (ex.: matrícula regular, transferência,
vestibulinho, bolsa). É uma informação da matrícula usada para relatórios e estatísticas.

---

## Quem pode utilizar

Secretaria e Administrador.

---

## Como acessar

```
Tabelas
→ Forma de Ingresso
```

---

## Quando utilizar

- Ao definir as maneiras pelas quais um aluno pode ingressar na escola.

---

## Como utilizar

1. Abra **Forma de Ingresso**.
2. Clique em **Novo**.
3. Informe o **Código** e a **Descrição**.
4. Clique em **Gravar**.

---

## Campos

| Campo | Descrição | Obrigatório |
|-------|-----------|-------------|
| Código | Código da forma de ingresso | Sim |
| Descrição | Nome da forma de ingresso | Sim |

---

## Botões

| Botão | O que faz |
|-------|-----------|
| **Novo** | Cadastra uma nova forma de ingresso |
| **Pesquisar** | Busca formas de ingresso |
| **Gravar** | Salva o registro |
| **Opções** | Editar/excluir |

---

## Resultados

- A forma de ingresso fica disponível para seleção na matrícula do aluno.

---

## Regras importantes

- **Código** e **Descrição** são obrigatórios.

---

## Fluxo da funcionalidade

```mermaid
flowchart TD
    A[Novo] --> B[Código e descrição]
    B --> C[Gravar]
    C --> D[Disponível na matrícula]
```

---

## Exemplos práticos

**Exemplo:** cadastrar "Matrícula Regular", "Transferência" e "Bolsa Integral".

---

## Dicas

- Padronize as formas para relatórios e estatísticas consistentes.

---

## FAQ

**Onde escolho a forma de ingresso?**
Na matrícula do aluno.

---

## Prints

- [ ] Lista de formas de ingresso.
- [ ] Formulário.

---

## Histórico

| Data | Alteração |
|------|-----------|
| 30/07/2026 | Página criada. |
