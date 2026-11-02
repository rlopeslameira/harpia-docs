# Contexto do projeto — Harpia Educacional

> Documento técnico interno. **Não** faz parte do manual do usuário (`docs/`) nem do índice da atendente IA.
> Arquitetura: [ARCHITECTURE.md](ARCHITECTURE.md) · Decisões: [DECISIONS.md](DECISIONS.md) ·
> Regras de trabalho: [../../CLAUDE.md](../../CLAUDE.md)

## Objetivo

Sistema web de gestão escolar — **administrativo, acadêmico (secretaria) e financeiro** — usado por escolas
(inclui cobrança de mensalidades). O Harpia é a evolução do **Mais Escola** (Visual FoxPro); existe um migrador de dados
Mais Escola (DBF) → Harpia (MySQL), mantido fora deste workspace.

## Modelo de operação

- **Multi-escola (tenant)**: cada escola tem um banco de dados próprio, identificado por um código informado no login.
- **Multi-empresa**: dentro de uma escola, os dados são separados por `EmpresaId` (unidade/escola do grupo).
- **Período letivo**: a maior parte dos dados acadêmicos e financeiros é por `Ano` + `Sequencial`.
  O usuário escolhe empresa e período após o login e pode trocá-los na topbar.

## Perfis de usuário

- **Usuários com perfil**: o acesso é definido por perfil, por item de menu, com as permissões
  `Visível`, `Incluir`, `Editar`, `Excluir` e `Imprimir`.
- **Master / SuperMaster**: acesso total; únicos que usam ferramentas internas (ex.: Gerenciador de Tabelas,
  reset financeiro em desenvolvimento).
- Funcionários são classificados como Administração, Professor e Coordenador (cadastro de funcionários).

## Módulos e funcionalidades

Organização espelhada no manual (`harpia-docs/docs/`):

| Módulo | Principais funcionalidades |
|---|---|
| Cadastro | Alunos (dados, pais/responsáveis, documentos, matrícula), funcionários, professores, visitantes |
| Acadêmico / Secretaria | Cursos, séries (nível/sequência), turmas, disciplinas e matérias, matriz curricular, grade (oferta de disciplinas), turmas auxiliares, estágios, situação do aluno, troca de turma |
| Notas e Frequência | Lançamento de notas por avaliação, faltas (mês/dia/global), conselho de classe |
| Boletim | Configuração (colunas, avaliações, convenções, conceitos, fórmulas), processamento e consulta |
| Histórico | Histórico escolar e observações |
| Financeiro | Planos de pagamento, mensalidades, recebimento/estorno, outros recebimentos, contas a pagar, caixa (abastecimento, sangria, encerramento), diário financeiro, cartões e faturas |
| Bancário | Boletos, remessa/retorno CNAB, baixa automática, integração Asaas |
| Relatórios | Relatórios em PDF e impressões (recibos, relações, diário de classe) |
| Documentos Word | Modelos `.docx` da escola preenchidos com dados do aluno (DOCX, PDF ou ZIP) |
| Configurações / Tabelas | Parâmetros, tabelas da secretaria e financeiras, usuários, perfis e menus |
| Dashboard | Widgets configuráveis (contadores, vagas, sintético por plano) |
| Atendente virtual (IA) | Chat que responde **como usar o sistema** a partir do manual — ver [../../ATENDENTE-IA-COMO-LIGAR.md](../../ATENDENTE-IA-COMO-LIGAR.md) |

## Regras de negócio globais

- Todo dado de escola é isolado pelo banco da escola; dentro dele, filtrar por `EmpresaId` e período quando aplicável.
- Textos são gravados em **MAIÚSCULAS** (conversão automática na API), exceto campos explicitamente excluídos
  (e-mail, senha, fórmulas, tokens, conteúdos ricos etc.).
- Datas e valores seguem o padrão brasileiro na interface (`dd/mm/aaaa`, R$).
- **Boletim — arredondamento**: o sistema não arredonda notas automaticamente; quando a escola quer arredondar,
  o arredondamento fica **na fórmula** (`ROUND(valor, casas)`). Ver [DECISIONS.md](DECISIONS.md) (D-001).
- O resultado acadêmico (médias, situação final, "precisa de") é **calculado pelo processamento do boletim**,
  não digitado.
- Matrícula de funcionário segue `8500000 + FuncionarioId`.
- A atendente IA não consulta dados da escola nem executa ações.

## Integrações relevantes

- **Asaas** — cobranças/boletos e webhook de pagamento.
- **Bancos (CNAB 240/400)** — arquivos de remessa e retorno; baixa automática.
- **ControlID** — eventos de catraca/controle de acesso (rotas `/controlid/*`).
- **Provedores de LLM** (Gemini, Groq, OpenAI, Anthropic ou Ollama, configurável) — apenas para a atendente IA.
- **ViaCEP / IBGE** — endereço e cidades no front.
- **Puppeteer** — geração de PDF de relatórios no servidor.

## Limitações conhecidas

- **API sem autenticação efetiva**: só o header do código da escola é exigido; as permissões são aplicadas no
  front. Prevista como etapa futura (ver análise do boletim no Projeto).
- O login compara e-mail e senha diretamente na consulta, sem hash no servidor (`UsuariosController.index`).
- Lógica pesada (ex.: processamento do boletim) roda no navegador, com muitas requisições por aluno; está em
  melhoria por etapas.
- Inconsistências pontuais de modelagem já mapeadas (ex.: faltas globais com `DisciplinaCodigo` vazio × `NULL`,
  significado de `TipoLancamentoFaltas`). Ver documentos do Projeto citados em [DECISIONS.md](DECISIONS.md).
- Telas sem documentação no manual: ver [../../LACUNAS-DOCUMENTACAO.md](../../LACUNAS-DOCUMENTACAO.md).
- Controllers do app (`path-api/app/Controllers/Http/appMe`) existem, mas as rotas estão desativadas.
