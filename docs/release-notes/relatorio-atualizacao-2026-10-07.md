# Relatório da Atualização — 07/10/2026

Atualização da documentação funcional do Harpia. Escopo: mudanças **visíveis ao usuário** entregues
entre **22/08/2026 e 06/10/2026** (≈140 commits no `harpia-ui` e ≈70 no `path-api`). Refatorações,
padronização interna de datas, ajustes de fonte/layout de boleto sem mudança de fluxo e a landing
page comercial foram ignorados.

## Novas páginas criadas

| Página | Conteúdo |
|--------|----------|
| `financeiro/consulta-de-caixas.md` | Nova tela **Consulta de Caixas** |
| `gerenciador/reset-financeiro-aluno.md` | Nova tela **Reset Financeiro do Aluno** (somente Master) |
| `release-notes/2026-09.md` e `2026-10.md` | Notas de versão de setembro e outubro |
| `release-notes/relatorio-atualizacao-2026-10-07.md` | Este relatório |

## Páginas atualizadas

- **Financeiro:** encerramento-caixa (reescrita), sangria, abastecimento-caixa, ajusta-saldo-final-dia,
  transferencia-fonte, entrada-cartao, fatura-cartao, fatura-cartao-quitada, contas-a-pagar,
  outros-recebimentos, recebimento-mensalidade, ficha-financeira, recibo, geracao-boletos, README do módulo.
- **Relatórios:** financeiros (Diário Financeiro), funcionarios (reescrita), secretaria (Alunos por Turma e
  Relação de Assinaturas), README.
- **Outros:** cadastro/alunos (botão Documentos do Word), documentos-word/impressao, gerenciador/usuarios
  (Operador de Caixa Master), gerenciador/README, getting-started/navegacao, intro.md (índice geral).

## Páginas removidas

Nenhuma. Foram removidas das páginas apenas funcionalidades descontinuadas: relatórios *Senhas* e *Censo*
de Colaboradores, campos *Cheque*, *Data de Crédito* e *Bom Para*.

## Funcionalidades alteradas

1. Controle de caixa: Encerramento como único ponto de gravação, Consulta de Caixas, aviso/bloqueio por caixa pendente, Operador de Caixa Master.
2. Sangria e Ajusta Saldo: somente dinheiro. Transferência de Fonte sem Data de Crédito.
3. Faturas de cartão: valor líquido, quadros de quitação/estorno, bloqueio com confirmação automática.
4. Contas a Pagar e Outros Recebimentos: valores líquidos, Desconto Extra, regra de Cortesia, recibo único.
5. Diário Financeiro: novos filtros, cores, etiquetas de fatura; totais removidos.
6. Boletos: novo carnê e aviso de bancos divergentes.
7. Relatórios de Colaboradores e Alunos por Turma reformulados.
8. Alunos: atalho Documentos do Word; Impressão com último modelo pré-selecionado.
9. Busca do menu sem acentos; calendário de período nas telas financeiras.

## Prints que precisam ser atualizados

- Novos: Consulta de Caixas; Reset Financeiro; aviso amarelo de caixa pendente; Encerramento (Ações/Status/Total Geral); Fatura Abertas (quadro de quitação e tela bloqueada); Fatura Quitadas (quadro de Estorno); janela de bancos divergentes; Funcionários (3 tipos e PDFs); Alunos por Turma com turma auxiliar; Alunos (botão Documentos do Word); Usuários (Operador de Caixa Master).
- Recapturar: Sangria; Ajusta Saldo; Transferência de Fonte; Outros Recebimentos (cortesia/colunas); Diário Financeiro (filtros e cores); carnê.

## Pendências encontradas

1. Reset Financeiro: a trava de "somente desenvolvimento" está comentada; hoje qualquer Master apaga dados reais.
2. Diário Financeiro perdeu os cartões de totais e o rodapé TOTAIS — confirmar se é intencional.
3. Relatório de Assinatura de Colaboradores ignora "Organizar por: Matrícula" (possível bug).
4. Documentos Word › Impressão: em escola sem último modelo gravado, pode aparecer "Não foi possível carregar os modelos e as turmas" (possível bug).
5. Outros Recebimentos: não há campo "Desconto Extra" no formulário (só em recibo/Diário).
6. Toasts duplicados de cartão ("Informe o cartão" + "Selecione o cartão utilizado no pagamento.") em Ficha/Recebimento.
7. Âncora `README.md#regras-gerais-do-módulo` em encerramento-caixa.md pode não funcionar no Docusaurus.
8. Build do Docusaurus não foi executado nesta rotina.
9. Integração Asaas segue sem tela própria — pendente desde 21/08.

## Dúvidas que precisam de validação

- Nome exato dos itens de menu de **Consulta de Caixas** e **Relação de Assinaturas** (vêm do banco de menus).
- A senha impressa no relatório Resumido de Colaboradores é armazenada em texto puro?
- Raiz do site mostrando a landing page é intencional para usuários com favoritos antigos?

## Sugestões de melhoria para a documentação

- Capturar os prints pendentes (maior lacuna: nenhum print novo desde julho).
- Criar uma página única "Regras de caixa aberto/fechado" e linkar de cada tela financeira.
- Documentar o calendário de período em uma página de componentes comuns.
- Rodar `npm run build` no `harpia-docs` para validar âncoras.
