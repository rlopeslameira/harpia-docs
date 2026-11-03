# Arquitetura — Harpia Educacional

> Documento técnico interno. Contexto de negócio: [PROJECT_CONTEXT.md](PROJECT_CONTEXT.md) ·
> Decisões: [DECISIONS.md](DECISIONS.md) · Regras por app: [path-api](../../path-api/CLAUDE.md),
> [harpia-ui](../../harpia-ui/CLAUDE.md)

## Visão geral

```
harpia-ui (React SPA) ──HTTP/JSON + headers Codigo/UsuarioId──▶ path-api (AdonisJS)
                                                                  │  Connections.getDbConnection(request)
                                                                  ▼
                                                    MySQL: 1 banco por escola (db<codigo>)
harpia-docs (Docusaurus) ──npm run ia:build──▶ path-api/resources/ia/indice-docs.json ──▶ /ia/chat
```

Três repositórios git independentes (branch de trabalho `dev`); a raiz do workspace não é versionada.

## Tecnologias

| Camada | Tecnologias |
|---|---|
| Backend `path-api` | Node ≥ 18, AdonisJS 5.9, TypeScript ~4.6, Lucid 18 (Query Builder, sem models), mysql2, yup, Puppeteer-core, docxtemplater/pizzip, Japa |
| Frontend `harpia-ui` | React 17, CRA (react-scripts 5), TypeScript ~4.6, PrimeReact 7 + PrimeFlex 3 + PrimeIcons, react-hook-form 7, react-router-dom 6, axios, date-fns, chart.js, FullCalendar, TinyMCE/Quill, xlsx |
| Manual `harpia-docs` | Docusaurus 3.10, React 19, Mermaid |
| Banco | MySQL (sessão com `STRICT_TRANS_TABLES`) |
| Deploy | Front: site estático (`harpia-ui/render.yaml`, rewrite SPA). API: `Procfile` (`node build/server.js`). |

## Estrutura dos principais diretórios

```
path-api/
  start/routes.ts            rotas (agrupadas por recurso; controller por string)
  start/kernel.ts            middlewares globais e nomeados
  app/Controllers/Http/      controllers (regra de negócio) + connections.ts (multi-tenant)
    reports/                 motor de relatórios PDF (builders, renderers, docs)
    boletos/  appMe/         boletos; controllers do app (rotas desativadas)
  app/Services/              Ia/, DocumentosWord/, Parametros/, Asaas, FaturaCartao, Puppeteer
  app/Middleware/            CodEscola, UpperCase, SanitizeDates, IaGuard (Auth existe, não registrado)
  config/database.ts         montagem das conexões por escola
  database/scripts/          SQL manual de schema/índices (aplicar em todos os bancos)
  scripts/ia, scripts/boletim  indexação da IA; verificação de boletim em produção
  docs/                      documentação técnica de relatórios, IA de schema, fechamento de fatura
  tests/functional, tests/unit
harpia-ui/src/
  config/Routes.tsx          rotas lazy
  pages/                     telas (Tela.tsx + TelaForm.tsx), MainLayout, AppTopbar, AppMenu
  components/                design system SC* (index.ts), Select*, dashboard, ia
  providers/                 auth (sessão), permissoes
  services/                  api.ts (axios), apiRelatorios, apiCep, apiIBGE, iaService
  templates/                 relatórios/impressões
  utilities/                 config, constantes, util, formulaEvaluator, financeiro…
  styles/theme.css           tokens de cor (fonte única)
harpia-docs/
  docs/                      manual do usuário (publicado e indexado pela IA)
  tecnico/                   documentação técnica (este diretório; não publicado)
```

## Comunicação entre as aplicações

- O front chama a API por `services/api.ts`; `baseURL` em `utilities/config.tsx`.
- Headers enviados em toda requisição: **`Codigo`** (escola, de `localStorage['harpia.codigo']`) e
  **`UsuarioId`** (de `localStorage['harpia.usuario']`). `Authorization` não é enviado (token vazio).
- Relatórios PDF usam um endpoint/base próprio (`baseReportUrl`, `services/apiRelatorios.ts`).

## Contratos de API relevantes

Convenções observadas (legado heterogêneo — manter o formato que a tela já consome):

- Rotas REST-like por recurso, sem versionamento: `GET/POST/PUT/DELETE /<recurso>` + ações nomeadas
  (`/<recurso>/<acao>`). Parâmetros de leitura e de exclusão vão em **query string**.
- Erro de validação: `400` com array de mensagens (yup `error.errors`); erro de regra: `400 { error: string }`.
- Sem header `codigo`: `401 { error: 'Codigo da escola nao informado!' }`.

Grupos com contrato próprio:

| Grupo | Rotas | Observação |
|---|---|---|
| Atendente IA | `GET /ia/status`, `GET /ia/sugestoes`, `POST /ia/chat` | `iaGuard`: exige `codigo` + `usuarioid`, limita por minuto/dia, `IA_ATIVO` |
| Schema p/ IA | `GET /ia/bd/{schema,tabelas,tabela,busca}` | só metadados; bloqueado em produção |
| Relatórios | `POST/GET /gerarRelatorio`, `POST /v3/report/{preview,pdf}` | payload em `path-api/app/Controllers/Http/reports/documentacao_relatorios.md` |
| Documentos Word | `/documentosWord*` | gera DOCX/PDF/ZIP a partir de modelo com `{{CAMPO}}` |
| Asaas | `POST /asaas/boleto`, `POST /asaas/webhook` | |
| Catraca | `POST /controlid/*` | |
| Dev | `/dev/resetFinanceiroAluno/*` | bloqueado em produção, fora de localhost e para não-Master |

## Autenticação e autorização

- Login: `UsuariosController.index` valida e-mail/senha e devolve o usuário; o front guarda usuário, empresa e
  período em `localStorage` (`providers/auth.tsx`).
- **Não há sessão/token validado pela API** (`@adonisjs/auth` instalado; middleware `Auth` não registrado).
- Autorização é feita **no front** (`providers/permissoes.tsx`): permissões por item de menu/rota
  (`Visivel/Incluir/Editar/Deletar/Imprimir`); Master/SuperMaster liberam tudo.
- Proteções no servidor existem apenas em rotas específicas (`iaGuard`, bloqueios por `NODE_ENV`).

## Banco de dados e persistência

- MySQL, **um banco por escola**. `config/database.ts` registra `db12345` (demonstração, a partir de
  `DATABASE_URL`) e varre `DATABASE_URL_<CODIGO>`; `Connections.getDbConnection` também registra sob demanda.
- Acesso por **Query Builder** (`dbCon.from(...)`, `dbCon.table(...).insert`, `dbCon.raw`) — sem ORM/models.
- Particionamento lógico dentro do banco: `EmpresaId` + `Ano` + `Sequencial` (`varchar`).
- Nomes de tabelas e colunas em PascalCase português (`AlunosPeriodo`, `OfertaDisciplina`, `NotaB1`).
- Schema de referência: `path-api/estruturaBD.sql`. Alterações via scripts SQL idempotentes em
  `database/scripts/`, aplicados manualmente em cada banco. Migrations do Adonis praticamente não são usadas.
- Transações usadas pontualmente em operações financeiras/acadêmicas de várias etapas.

## Padrões arquiteturais existentes

- **Controller-centric**: regra de negócio no controller; services para lógica reutilizável/isolável.
- **Middlewares globais de normalização** (`UpperCase`, `SanitizeDates`) — impactam todo campo novo.
- **Front: par lista + formulário** por entidade, design system `SC*` sobre PrimeReact + react-hook-form,
  tokens de cor em `theme.css`.
- **Processamentos pesados no cliente** (ex.: boletim), com plano de migração para services no servidor.
- **Documentação do usuário como fonte da IA**: manual padronizado → índice local → RAG na API.
