# Relatório da Atualização — 07/08/2026

Atualização semanal da documentação funcional do Harpia. Escopo do período: mudanças **visíveis
ao usuário** entregues entre **01/08/2026 e 07/08/2026**, concentradas no **módulo Financeiro**.

> Base comparada: última geração da documentação em 31/07/2026. Foram analisados os commits das
> aplicações (`harpia-ui` e `path-api`) posteriores àquela data, filtrando apenas o que **muda a
> experiência do usuário** (telas, campos, botões, mensagens, regras e relatórios). Refatorações,
> ajustes de tipos, correções de timezone e mudanças internas foram ignorados conforme a regra de ouro.

## Novas páginas criadas

- `docs/release-notes/2026-08.md` — notas de versão de agosto/2026.
- `docs/release-notes/relatorio-atualizacao-2026-08-07.md` — este relatório.

Nenhuma nova página de funcionalidade foi necessária: as mudanças do período incidiram sobre
telas já documentadas.

## Páginas atualizadas

- `docs/financeiro/recibo.md` — remoção da marca **REEMISSÃO**; nova linha **Total**; **máquina/tipo
  do cartão** na Forma de Pagto.; turma com descrição e cidade/UF dinâmicas.
- `docs/financeiro/geracao-boletos.md` — **novo modelo de boleto** (layout/logo); campo **Layout de
  impressão** (1 ou 3 boletos por página); dados bancários (Agência/Carteira) em tela; **dedução** no
  valor; filtro obrigatório (Curso/Turma/Aluno); novas mensagens.
- `docs/financeiro/ficha-financeira.md` — nova coluna **Banco**; **selo M** na parcela de matrícula;
  atualização automática ao receber/estornar a matrícula.
- `docs/financeiro/outros-recebimentos.md` — nova coluna **Valor Pago** na lista; recibo com Total e
  máquina do cartão.
- `docs/financeiro/recebimento-mensalidade.md` — recibo com Total e máquina do cartão; nota sobre
  correção do recebimento de adiantamento.
- `docs/financeiro/contas-a-pagar.md` — correção da exclusão de contas pagas com cartão institucional.

## Páginas removidas

- Nenhuma.

## Funcionalidades alteradas (com impacto ao usuário) no período

- **Recibo:** deixou de exibir a marca **REEMISSÃO** nas 2ª vias (mantendo o mesmo número); ganhou a
  linha **Total** (mensalidade + acréscimos + multas + juros − desconto − dedução − desconto extra);
  passou a mostrar a **máquina/tipo do cartão** (Crédito/Débito/Parcelado/PIX) na Forma de Pagto.; e
  exibe a **turma com descrição** e a **cidade/UF** vindas do cadastro da escola.
- **Geração de Boletos:** **novo modelo** com layout e **logo** atualizados; novo campo **Layout de
  impressão** com **1 boleto por página** ou **3 boletos por página** (obrigatório); exibição de
  **Agência/Carteira** do banco para conferência; valor do boleto passa a considerar a **dedução**;
  nova validação *"Selecione ao menos um filtro: Curso, Turma ou Aluno."* e *"Seleção do número de
  páginas para impressão é obrigatória."*.
- **Ficha Financeira:** nova coluna **Banco** na tabela de parcelas; **selo M** na parcela de
  matrícula; atualização automática da consulta do aluno ao receber/estornar a matrícula.
- **Outros Recebimentos:** nova coluna **Valor Pago** na lista.
- **Contas a Pagar:** correção da **exclusão** de contas pagas com **cartão institucional** — a taxa
  do cartão vinculada é removida junto.

## Prints que precisam ser atualizados / capturados

As capturas anteriores do **recibo com REEMISSÃO** e do **boleto no modelo antigo** estão
**desatualizadas** e precisam ser refeitas. Prioridades:

1. **Recibo** — mensalidade e outros recebimentos, agora com a linha **Total** e **sem** a marca REEMISSÃO.
2. **Recibo (cartão)** — Forma de Pagto. exibindo o tipo da máquina (Crédito/Débito/Parcelado/PIX).
3. **Geração de Boletos** — tela com o novo campo **Layout de impressão**.
4. **Boleto** — **novo modelo** (logo atualizado), em **1 por página** e em **3 por página**.
5. **Ficha Financeira** — tabela com a nova coluna **Banco** e o **selo M** na parcela de matrícula.
6. **Outros Recebimentos** — lista com a nova coluna **Valor Pago**.

Salvar as imagens em `docs/assets/images/` e inseri-las na etapa correspondente de cada página.

## Pendências encontradas

- **Screenshots** continuam inexistentes; as páginas mantêm checklists indicando onde inserir e quais recapturar.
- **Modelo de carnê x novo modelo de boleto:** confirmar como o novo modelo convive com o modelo de carnê dos bancos layout 60.
- **Extração do tipo da máquina:** o recibo deduz o tipo (Crédito/Débito/Parcelado/PIX) a partir da descrição da máquina; validar se todas as máquinas cadastradas seguem esse padrão.

## Dúvidas que precisam de validação

- **REEMISSÃO:** confirmar se a remoção da marca é definitiva ou se haverá um controle/opção para reexibi-la em auditorias.
- **Layout de impressão:** confirmar se "3 boletos por página" se aplica a todos os bancos/layouts ou apenas ao modelo padrão.
- **Dedução no boleto:** validar as regras de negócio de quando a dedução deve abater o valor do boleto.

## Sugestões de melhoria para a documentação

- Adicionar um **glossário financeiro** (máquina/adquirente, dedução, desconto extra, Nosso Número, carnê).
- Criar um **guia de captura de prints** (resolução/tema/nomenclatura) para padronizar as imagens pendentes.
- Iniciar a documentação dos módulos **não-financeiros** (Alunos, Turmas, Secretaria, Acadêmico, Boletim), ainda sem cobertura funcional.

## Verificação final

- ✓ Links relativos entre documentos: **0 quebrados** nas páginas editadas.
- ✓ Blocos de admonição (`:::note` / `:::tip` / `:::info`) balanceados nas páginas alteradas.
- ✓ Índice/histórico de cada página atualizado com a data 07/08/2026.
- ✓ Nenhuma página órfã criada; os arquivos entram na categoria **release-notes** já existente.
- ⚠ Imagens: marcadores presentes, arquivos ainda não capturados (pendência acima).

---

_Gerado automaticamente pela tarefa semanal de atualização de documentação em 07/08/2026._
