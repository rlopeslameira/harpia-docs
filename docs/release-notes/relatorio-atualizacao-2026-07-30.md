# Relatório da Atualização — 30/07/2026

Atualização semanal da documentação funcional do Harpia. Escopo do período (jul/2026): **módulo Financeiro**.

> Contexto: esta é a **primeira** geração da documentação funcional (`docs/`). Antes existiam
> apenas artefatos técnicos em `path-api/docs` (modelos de relatório, SQL). Por isso, além de
> atualizar, esta rodada **estruturou** a base de documentação e cobriu as telas efetivamente
> alteradas nas últimas semanas.

## Novas páginas criadas

- `docs/README.md` — índice geral
- `docs/getting-started/introducao.md`
- `docs/getting-started/navegacao.md`
- `docs/financeiro/README.md` — visão geral do módulo
- `docs/financeiro/recebimento-mensalidade.md`
- `docs/financeiro/outros-recebimentos.md`
- `docs/financeiro/contas-a-pagar.md`
- `docs/financeiro/geracao-boletos.md`
- `docs/financeiro/arquivo-remessa.md`
- `docs/financeiro/ficha-financeira.md`
- `docs/financeiro/zerar-mensalidades.md`
- `docs/bancario/bancos.md`
- `docs/faq/financeiro.md`
- `docs/release-notes/2026-07.md`

## Páginas atualizadas

- Nenhuma pré-existente (primeira edição da documentação funcional).

## Páginas removidas

- Nenhuma.

## Funcionalidades alteradas (com impacto ao usuário) no período

- Recebimento de mensalidade com **valor a menor** + **distribuição de valores**.
- **Recibos padronizados** com **numeração** (mensalidade e outros recebimentos), número no **estorno**.
- Outros Recebimentos: **pesquisa de aluno padronizada**, telas de **Cartão/Entrada de Cartão**.
- Contas a Pagar: **centro de custo padrão**, **alterar credor**, correções de estorno/alteração.
- Geração de Boletos: **banco do cadastro da turma**, ajustes de valores/estilo.
- Arquivo de Remessa: **validação de layout**, validações de **CNPJ do banco** e **CPF do responsável**.
- Cadastro de Bancos: ajustes de campos + **combo de bancos em Turmas**.
- Ficha Financeira: recálculos + **data de vencimento do saldo**.
- Diário Financeiro: consulta e correção de duplicidade de REF.

## Prints que precisam ser capturados

Todas as páginas do módulo têm marcadores de print (checklist). Prioridade de captura:

1. Recebimento de Mensalidade — tela principal + janela de **distribuição de valores** + recibo.
2. Geração de Boletos — filtros + visualização do boleto.
3. Arquivo de Remessa — filtros + mensagem de sucesso.
4. Contas a Pagar — formulário + **Alterar Credor**.
5. Outros Recebimentos — lista + formulário + recibo.
6. Ficha Financeira — tabela de parcelas.
7. Zerar Mensalidades — filtros + painel "O que Fazer?".
8. Cadastro de Bancos — abas Principal/Configurações/Mensagens.

As imagens devem ser salvas em `docs/assets/images/` e inseridas na etapa correspondente de cada página.

## Pendências encontradas

- **Screenshots ainda não existem** — nenhuma imagem foi capturada; os marcadores indicam onde inseri-las.
- **Menu real por escola** — o menu é carregado do banco (API `/menu`), configurável por tenant.
  Os caminhos documentados seguem as rotas do sistema; os rótulos/posições exatos precisam ser
  confirmados na instalação de referência.
- **Módulos não-financeiros** (Alunos, Turmas, Secretaria, Acadêmico, Boletim, Diário, Comunicação,
  Relatórios, Configurações) ainda **não possuem documentação funcional**. Não houve mudança
  relevante neles no período, mas a base está vazia e deve ser preenchida em rodadas futuras.

## Dúvidas que precisam de validação

- **Permissões por tela:** a seção "Quem pode utilizar" foi preenchida com os perfis prováveis
  (Financeiro/Administrador/Secretaria). A matriz exata de permissões por perfil deve ser confirmada.
- **Rótulos de menu:** confirmar os nomes exatos dos itens e grupos do menu na escola padrão.
- **Regras de negócio de borda** (ex.: reabertura de caixa, quem pode estornar) — validar com a equipe.

## Sugestões de melhoria para a documentação

- Padronizar a captura de prints (mesma resolução/tema) e criar um guia de nomenclatura de imagens.
- Publicar a base em **Docusaurus/MkDocs** para navegação e busca (a estrutura já é compatível).
- Criar `docs/getting-started` mais completo (login, perfis, primeiro acesso).
- Iniciar a documentação dos módulos acadêmico/secretaria nas próximas rodadas.
- Adicionar um glossário central de termos financeiros.

---

_Gerado automaticamente pela tarefa semanal de atualização de documentação em 30/07/2026._
