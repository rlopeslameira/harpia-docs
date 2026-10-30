# Relatório da Atualização — 30/10/2026

Atualização da documentação funcional do Harpia. Escopo: mudanças **visíveis ao usuário** entregues
entre **07/10/2026 e 30/10/2026** (≈60 commits no `harpia-ui` e ≈30 no `path-api`). Refatorações,
code splitting, ajustes de build/deploy, conexões de banco por variável de ambiente e correções internas de cálculo foram ignorados.

## Novas páginas criadas

| Página | Conteúdo |
|--------|----------|
| `release-notes/relatorio-atualizacao-2026-10-30.md` | Este relatório |

Complemento (mesma data): criada `getting-started/imprimir-relacoes.md` (guia único de impressão, ligado às 5 telas) e terminologia **ícones da linha** padronizada em convenio, fonte-financeira, forma-recebimento, ocorrencia-estorno, codigo-ocorrencia e funcionarios.

Os novos relatórios foram documentados como seções dentro de páginas existentes (ver abaixo).

## Páginas atualizadas

- **Financeiro:** transferencia-fonte, ajusta-saldo-final-dia, entrada-cartao, fatura-cartao, abastecimento-caixa, sangria (botão Opções).
- **Tabelas Financeiras:** cartao-institucional (Dia de Fechamento da Fatura), plano-pagamento, plano-contas, cartao, centro-custo, feriado-financeiro (Impressão / remoção do botão Opções).
- **Relatórios:** financeiros (Movimentação Financeira e Outros Recebimentos), secretaria (Responsáveis), README.
- **Acadêmico / Cadastro / Tabelas de Secretaria:** turmas (Turma ativa + Imprimir), disciplinas (Imprimir), turmas-auxiliares (Alunos x Turma Auxiliar), cursos, itinerario-formativo, materias, nivel-sequencia, documentos-matricula, origem.
- **Outros:** release-notes/2026-10.md, intro.md (índice geral).

## Páginas removidas

Nenhuma.

## Funcionalidades alteradas

1. Transferência de Fonte e Ajusta Saldo Final do Dia: lista (consulta) + cadastro em janela (Novo / Gravar / Cancelar); Observação obrigatória no ajuste.
2. Entrada de Cartão: tela redesenhada (Cartão obrigatório, Confirmação, seleção, quadro Confirmação / Resumo, mensalidades em cartão).
3. Fatura de Cartão: filtro Fechamento (mês/ano), agrupamento por fechamento, coluna Bom Para; Cartão Institucional com Dia de Fechamento da Fatura.
4. Novos relatórios: Movimentação Financeira, Outros Recebimentos, Responsáveis, Turmas, Disciplinas, Plano de Pagamento, Plano de Contas, Alunos x Turma Auxiliar.
5. Turmas: campo Turma ativa. Abastecimento de Caixa e Sangria: filtro único Data Inicial/Final.
6. Botão Opções do topo removido de ~19 telas de cadastro/financeiro.

## Prints que precisam ser atualizados

- Novos: Transferência de Fonte (lista e janela); Ajusta Saldo (lista com agrupamento Saldo e janela); Entrada de Cartão (tela completa); Fatura Abertas (filtro Fechamento e lista agrupada); Cartão Institucional (campo Dia de Fechamento); Movimentação Financeira (filtros, prévia, aviso de dias sem saldo); Relação de Outros Recebimentos; Responsáveis; janelas "Filtro para impressão" de Turmas, Disciplinas, Plano de Pagamento, Plano de Contas e Turmas Auxiliares; cadastro de Turmas (Turma ativa).
- Recapturar: telas de cadastro que perderam o botão Opções (Cursos, Matérias, Cartão, Centro de Custo etc.); Abastecimento e Sangria (filtros).

## Pendências encontradas

1. As 9 pendências do relatório de 07/10 continuam sem verificação nesta rodada (Reset Financeiro sem trava, totais do Diário, Assinatura de Colaboradores, etc.).
2. Os commits de **Relação Outros Recebimentos** estão marcados "em andamento"; a documentação reflete a tela atual e pode mudar.
3. Nome exato dos itens de menu dos novos relatórios (Movimentação Financeira, Outros Recebimentos, Responsáveis) não foi confirmado no cadastro de menus.
4. Várias páginas descreviam "Opções" como ação de linha (editar/excluir); foi ajustado nas telas que perderam o botão, mas outras páginas ainda usam o termo (ex.: cadastro/alunos, enderecos, visitantes, setor, perfil) — revisar.
5. Fatura de Cartão Quitada recebeu alteração mínima no código; não foi revalidada. A página `fatura-cartao-quitada.md` não foi alterada.
6. Build do Docusaurus não foi executado nesta rotina (verificação de links feita por script).

## Dúvidas que precisam de validação

- Entrada de Cartão: a **Data de Crédito** não é mais digitada na confirmação (usa a data programada do lançamento) — confirmar que é o comportamento desejado.
- O período máximo de 366 dias na Movimentação Financeira é regra de negócio definitiva?
- Fatura de Cartão: o que fazer com lançamentos "Pendentes de conferência" (sem Bom Para)? Hoje só são agrupados à parte.

## Sugestões de melhoria para a documentação

- Capturar de fato os prints (ainda são apenas marcadores na documentação).
- ~~Página única de impressão~~ (feito). ~~Padronizar "ícones da linha"~~ (feito nas telas sem botão Opções; nas que ainda têm o menu Opções, como Alunos, Endereços, Setor, Perfil e Visitantes, a descrição "Editar/excluir" precisa ser conferida tela a tela).

## Complemento — prints capturados (30/10/2026)

Foram capturados 22 prints direto do sistema (ambiente de teste, empresa Centro Educacional Encantado, versão 1.0.59) e inseridos nas etapas correspondentes. Arquivos em `docs/assets/images/<módulo>/`:

- **financeiro:** transferencia-fonte-lista/novo, ajusta-saldo-lista/novo, entrada-cartao, fatura-cartao-abertas, abastecimento-caixa, sangria.
- **tabelas-financeiras:** cartao-institucional-lista/novo, plano-pagamento-filtro-impressao, plano-contas-filtro-impressao.
- **relatorios:** movimentacao-financeira-filtros/previa, outros-recebimentos-filtros, responsaveis-filtros.
- **academico / cadastro:** turmas-lista, turmas-menu-opcoes, turmas-filtro-impressao, turmas-form-principal, disciplinas-filtro-impressao, turmas-auxiliares-filtro-impressao.

Ajustes de texto feitos a partir do que a tela mostra: Responsáveis tem chaves de **Situação**; Outros Recebimentos usa o rótulo **Receitas** (padrão *Recebidas*); a Fatura de Cartão informa "Limpe o mês para conferir todas as faturas abertas."; Plano de Pagamento abre com Parcelas = Não e Contas = Sim.

**Ainda sem print (ambiente de teste sem dados):** Fatura com lançamentos agrupados e Bom Para, Entrada de Cartão com lançamentos marcados, Movimentação Financeira com lançamentos reais, PDFs gerados, aviso amarelo de caixa pendente e telas de telas já documentadas em rodadas anteriores (cerca de 264 marcadores `- [ ]` no manual).

**Rodada com dados de 29/10:** recapturados a prévia da Movimentação Financeira (4 lançamentos, totais por forma e resumo de saldos) e a Entrada de Cartão com lançamentos marcados (VISA; nada foi confirmado). A Fatura de Cartão (ITAU BLACK, 10/2026) retornou vazia, então o print dela com grupos de fechamento e Bom Para continua pendente.
