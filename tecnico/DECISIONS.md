# Registro de decisões técnicas — Harpia Educacional

> Documento técnico interno. Contexto: [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) · Arquitetura: [ARCHITECTURE.md](ARCHITECTURE.md)

## Como usar

- Registre apenas decisões **confirmadas** (pelo Rodrigo ou já implementadas com origem rastreável).
- Nunca apague uma decisão: ao mudar, crie uma nova e marque a antiga como `Substituída por D-XXX`.
- Status: `Proposta` · `Ativa` · `Substituída` · `Revogada`.
- Modelo:

```
## D-XXX — Título curto
- Data: AAAA-MM-DD · Status: Ativa · Origem: quem decidiu / onde está registrado
- Contexto: por que a decisão foi necessária.
- Decisão: o que foi decidido.
- Impactos e restrições: o que isso obriga ou impede.
```

Documentos-fonte fora deste repositório (Projeto "Harpia Educacional" no Claude):
`boletim/analise-processamento-boletim.md`, `boletim/correcoes-processamento-boletim.md`,
`migrador/decisoes-mapeamento-maisescola-harpia.md`.

---

## D-001 — Boletim: sem arredondamento automático
- Data: 2026-10-31 · Status: Ativa · Origem: Rodrigo (`boletim/correcoes-processamento-boletim.md`, passo 5)
- Contexto: notas calculadas eram comparadas sem regra clara de arredondamento (item A7 da análise).
- Decisão: o sistema não arredonda nada automaticamente; quando a escola quiser, o arredondamento fica **na fórmula**.
  `ROUND(valor, casas)` passou a aceitar casas decimais; `ROUND(valor)` continua arredondando para inteiro.
- Impactos e restrições: a exibição em 2 casas (`toFixed(2)`) é só formatação e não entra nas regras.
  Documentado ao usuário em *Configurações › Fórmulas de Secretaria*.

## D-002 — Carga horária do boletim em `SMALLINT`
- Data: 2026-10-29 (migrador) / 2026-10-31 (script) · Status: Ativa — aplicação nos bancos pendente ·
  Origem: Rodrigo (`migrador/decisoes-...md`; `boletim/correcoes-...md`)
- Contexto: `Boletim.CargaHoraria` e `BoletimProcessado.CargaHoraria` eram `tinyint` (máx. 127), mas
  `OfertaDisciplina.Chr` chega a 240; com `STRICT_TRANS_TABLES` a gravação falha.
- Decisão: alterar as colunas para `SMALLINT` via `path-api/database/scripts/boletim_cargahoraria_smallint.sql` (idempotente).
- Impactos e restrições: aplicar em **todos os bancos de escola antes** de publicar a nova versão do processamento.

## D-003 — Atendente IA: escopo e local de execução
- Data: 2026-08-20 · Status: Ativa · Origem: [../../PLANO-ATENDENTE-IA.md](../../PLANO-ATENDENTE-IA.md) (seção 2)
- Decisão:
  - responde **apenas sobre o uso do sistema**, com base no `harpia-docs` + ajuda contextual da tela;
  - **não consulta dados do banco e não executa ações**;
  - documentação e índice ficam locais; só o trecho relevante e a pergunta vão para o LLM em nuvem;
  - o serviço roda **dentro do `path-api`** (controller + services em `app/Services/Ia`).
- Impactos e restrições: o índice é o mesmo para todas as escolas; mudanças no manual exigem `npm run ia:build`.

## D-004 — Atendente IA: proteção da rota sem auth global
- Data: 2026-08-20 · Status: Ativa (provisória até existir autenticação na API) · Origem: `PLANO-ATENDENTE-IA.md`
  (seção 1); implementado em `path-api/app/Middleware/IaGuard.ts`
- Contexto: a API não tem autenticação efetiva e cada chamada de IA gera custo.
- Decisão: rotas `/ia` exigem `codigo` + `usuarioid`, têm limite por usuário/minuto e por escola/dia e respeitam `IA_ATIVO`.
  A chave do provedor fica só no `.env`.
- Impactos e restrições: contador em memória por instância (aceito para conter abuso acidental; Redis se precisar ser exato).

## D-005 — Atendente IA: contexto de tela pela rota
- Data: 2026-08-20 · Status: Ativa · Origem: `PLANO-ATENDENTE-IA.md` (seção 1)
- Contexto: a constante `tarefa` das páginas é inconsistente (copiada entre telas).
- Decisão: a chave do mapa tela→documentação é a rota do React Router (`useLocation().pathname`).
- Impactos e restrições: a rota enviada para `/ia/*` não pode ser alterada (por isso `/ia/` é exceção no `UpperCase`).

## D-006 — Conexões de escola por variável de ambiente
- Data: 2026-10-29 · Status: Ativa · Origem: commit `f27f3a9` (path-api)
- Decisão: cada escola é configurada por `DATABASE_URL_<CODIGO>`; a conexão `db<codigo>` é registrada no boot ou sob demanda.
- Impactos e restrições: incluir escola não exige alteração de código; o código vem do header `codigo`.

## D-007 — Migração Mais Escola → Harpia: regras de mapeamento
- Data: 2026-10-29/30 · Status: Ativa · Origem: Rodrigo (`migrador/decisoes-mapeamento-maisescola-harpia.md`)
- Decisão: o conjunto de regras de mapeamento campo a campo está no documento-fonte e não é duplicado aqui.
  Destaques: destino é um banco novo só do cliente; reexecução não duplica (chave natural/upsert);
  o resultado do processamento do boletim do Mais Escola **não** é migrado — o boletim é reprocessado no Harpia.

## D-008 — Local da documentação técnica
- Data: 2026-11-01 · Status: **Proposta — confirmar** · Origem: configuração do contexto permanente do workspace
- Contexto: `harpia-docs/docs/` é publicado e indexado pela atendente IA; documentação técnica ali chegaria ao usuário final.
- Decisão proposta: documentação técnica em `harpia-docs/tecnico/` (versionada no repositório do manual, fora do
  Docusaurus e do índice da IA); instruções para agentes em `CLAUDE.md` na raiz e em cada aplicação.

## D-009 — Perfil de tarefas: permissões gravadas conforme a tela
- Data: 2026-11-02 · Status: Ativa · Origem: Rodrigo
- Contexto: ao cadastrar um perfil, tarefas não marcadas eram enviadas sem valor e o banco gravava `1` (default de
  `PerfilMenu`), liberando telas que apareciam desmarcadas.
- Decisão: o front envia `Visivel/Incluir/Editar/Deletar` sempre como booleano — desmarcado na tela = sem permissão.
  Na edição, tarefas ainda sem registro em `PerfilMenu` continuam vindo **liberadas** da API (`GET /perfil`) — comportamento confirmado.
- Impactos e restrições: `Imprimir` não aparece na tela e segue o comportamento anterior. Perfis já gravados não são alterados.

## D-010 — Barra de ações de formulário fixa na base
- Data: 2026-11-02 · Status: Ativa · Origem: Rodrigo
- Decisão: em todas as telas, a barra com Cancelar/Gravar (e Excluir/Fechar) fica sempre visível na base da área
  rolável — do diálogo ou da página — mesmo com scroll. Implementação padrão: `SCFormActions`
  (`harpia-ui/src/components/form/formActions/`), `position: sticky; bottom: 0`. O `footer` do `SCDialog` também atende.
- Impactos e restrições: formulários novos e revisados devem usar o componente; telas com barras em layout próprio
  migram conforme forem tocadas.


## D-011 — Relatório Lançamentos de Entrada de Caixa segue o critério do Encerramento de Caixa
- Data: 2026-11-16 · Status: Ativa · Origem: Rodrigo
- Contexto: a primeira versão listava todo `DiarioFinanceiro` com `Opcao = 'INC'`, o que incluía pagamentos de Contas a
  Pagar (`Tarefa = 'APAGAR'`), filtrava por Ano/Sequencial e escondia recebimentos estornados depois — não batia com o caixa do dia.
- Decisão: entrada = `Tarefa` em `OUTPAG, BAIAUT, RECPAG, RECADI, RECMEN` com `Opcao = 'INC'`, pelo dia do lançamento, sem
  filtro de período letivo e sem taxa de cartão; o estorno de recebimento (`Opcao = 'EST'`) aparece no dia em que foi feito,
  com valor negativo, e o recebimento original permanece no dia dele (marcado como estornado). APAGAR (inclusive estornos)
  e TRFFON ficam de fora. BAIAUT recompõe o valor pago com multa, juros e complementares. O relatório gera PDF e Excel
  e **não** é bloqueado pelo parâmetro `PER_DIACAIXA` (caixa aberto).
- Implementação: `path-api/app/Services/Relatorios/LancamentosEntradaCaixaService.ts` (lista de tarefas espelha
  `harpia-ui/src/utilities/calculoCaixa.ts` — mudar uma exige mudar a outra).

## D-012 — Filtros do relatório Lançamentos de Entrada de Caixa (paridade com o Mais Escola)
- Data: 2026-11-16 · Status: Ativa · Origem: Rodrigo (complementa D-011)
- Decisão: a tela reproduz os filtros do relatório do Mais Escola, exceto Convênio, ordenação por Campus/Sala e por
  Banco, tipo Detalhado/Listagem/Planilha, "Selecionar TODAS as bandeiras" e "Agrupar por Fonte/Forma".
  Regras confirmadas: **Considerar** Mensalidades = `RECPAG, RECADI, RECMEN, BAIAUT`, Outros = `OUTPAG`; **Baixas** =
  Todas / Só baixa automática (`BAIAUT`) / Sem baixa automática; **Exibir somente estornos com a mesma data de
  lançamento** (marcado por padrão) mantém o estorno só quando o recebimento original foi lançado no mesmo dia do
  estorno — com a opção marcada, estornos de recebimentos de outros dias não aparecem e o total do dia pode diferir do
  Encerramento de Caixa; **Exibir Nome do Responsável** = responsável financeiro (`AlunosResponsavel.Tipo = 'FIN'`).
- Mapeamento: Código Pagto. = `PlanosPagamentoId`; cartão (Máquina - Banco - Bandeira) = `CartoesId`; Banco da geração
  do boleto = `Mensalidades.BancoId` da mensalidade do lançamento; Conta = `ContaValorId`; Operador aceita vários.

## D-013 — Botões padronizados das telas de relatório
- Data: 2026-11-18 · Status: Ativa · Origem: Rodrigo (confirmado na conversa da tarefa)
- Contexto: as telas em `harpia-ui/src/pages/Impressao/Relatorio_*.tsx` misturavam botões só com ícone e tooltip
  (`react-icons` `SlPrinter`/`SiMicrosoftexcel`), cores fixas (`#0772B3`, `bg-green-400/500`), rótulos divergentes
  ("Imprimir", "Gerar Relatório", "Visualizar", "Excel") e larguras inline.
- Decisão: adotar **rótulo + ícone PrimeIcons**, cores exclusivamente por variáveis de `theme.css` e um componente único
  de design system: `SCRelatorioActions` (`harpia-ui/src/components/form/relatorioActions/`), exportado por `components/index.ts`.
  Padrão: **Exportar Excel** (`pi pi-file-excel`, `--success`), **Imprimir** (`pi pi-print`, `--primary`) e
  **Gerar Relatório** (`pi pi-eye`, `--info`, ação principal/submit). Cada ação só é renderizada quando informada
  (`excel` / `imprimir` / `gerar`), recebendo `onClick`, `type`, `disabled`, `loading` e `tooltip` do chamador.
- Impactos e restrições: aplicado a todas as telas `Relatorio_*`; os fluxos com prévia passaram a usar "Gerar Relatório"
  no lugar de "Visualizar". Botões de filtro (Pesquisar/Limpar) e de diálogo (Fechar/Cancelar/Confirmar) não fazem
  parte deste componente. Não altera contratos de API nem o manual do usuário.
