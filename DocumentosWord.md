# Documentos Word — instalação e uso

Função que permite à escola cadastrar documentos do Word com códigos de campo e
imprimi-los preenchidos com os dados de um aluno ou de uma turma inteira.

O sistema **não cria** o documento: ele abre o `.docx` que a escola enviou, troca os
códigos `{{...}}` pelos dados do aluno e devolve o mesmo arquivo preenchido — ou o PDF
dele, ou uma página pronta para imprimir. Toda a formatação do modelo (fontes, margens,
cabeçalho, rodapé, tabelas, logo) é preservada.

---

## 1. Instalação

### 1.1 Banco de dados

Execute uma vez **em cada base** (`harpia`, `harpiademo`, `encantado`, ...):

```
path-api/database/scripts/documentos_word.sql
```

O script cria as tabelas `DocumentosWord` e `DocumentosWordCampos`, carrega o catálogo
com ~130 campos, insere o menu "Documentos Word" (SistemaId = 1) com os 3 itens e
libera as telas para todos os perfis existentes.

> Se preferir liberar as telas manualmente pela tela de Perfis, não execute o
> bloco 5 do script.

### 1.2 Dependências do backend

```bash
cd path-api
pnpm install          # ou npm install / yarn
```

Foram adicionadas ao `package.json`:

| Pacote | Para quê | Licença |
|---|---|---|
| `docxtemplater` | abre o `.docx` e troca os `{{CAMPOS}}` | MIT |
| `pizzip` | leitura/escrita do zip que é o `.docx`, e geração do `.zip` de saída | MIT |
| `docx-preview` | desenha o `.docx` dentro do navegador, para gerar PDF e HTML | MIT |
| `jszip` | exigido pelo `docx-preview` | MIT |

### 1.3 PDF e HTML — nada a instalar no servidor

O PDF e a visualização em HTML saem do **mesmo Chrome que o sistema já usa nos
relatórios** (`app/Services/PuppeteerService.ts`). O `docx-preview` desenha o `.docx`
dentro dessa página respeitando os estilos do próprio arquivo, e daí saem as duas
saídas: `page.pdf()` para o PDF e `page.content()` para o HTML já renderizado.

Ou seja: **não é preciso instalar LibreOffice nem nada além do `pnpm install`.**
Se o Chrome mudar de lugar no servidor, o caminho é o mesmo do `PuppeteerService`:

```ts
// app/Services/PuppeteerService.ts
return isProduction
  ? "/app/.chrome-for-testing/chrome-linux64/chrome"
  : "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe";
```

A fidelidade é muito boa — fontes, margens, tabelas, cabeçalho e rodapé do modelo são
respeitados —, mas o desenho é do navegador, não do Word. Em documentos com recursos
raros de diagramação pode haver pequena diferença em relação ao que o Word mostra. O
`.docx` continua sendo a versão fiel: quem precisa de layout exato imprime por ele.

---

## 2. As três telas

| Tela | Caminho | O que faz |
|---|---|---|
| Modelos de Documentos | `/DocumentosWord/Modelos` | cadastra a descrição e envia o `.docx` |
| Impressão | `/DocumentosWord/Impressao` | escolhe modelo + aluno/turma e gera o arquivo |
| Campos Disponíveis | `/DocumentosWord/Campos` | consulta os códigos; clique copia `{{CODIGO}}` |

### Cadastro

Descrição + upload do `.docx`. Ao gravar, o sistema lê os códigos usados dentro do
modelo e avisa se algum não existe no catálogo — esse código sairia **em branco** na
impressão.

### Impressão

1. **Modelo** — dropdown com os modelos cadastrados
2. **Imprimir para** — `Turma` (carrega todos os alunos, já marcados) ou `Aluno`
   (usa o `ConsultaAluno`, o mesmo componente de busca das demais telas)
3. **Saída** — arquivo único (um aluno por página) ou um arquivo por aluno em `.zip`
4. **Formato**:
   - **Word (.docx)** — baixa e abre no Word
   - **PDF** — baixa pronto para arquivar ou enviar
   - **Visualizar / Imprimir** — mostra o documento já paginado num modal, com
     botões *Imprimir* e *Baixar HTML*
5. **Gerar documento**

No modo Aluno a busca traz alunos de **qualquer situação** (transferido, cancelado),
já que declarações e históricos são pedidos justamente por quem saiu. No modo Turma
vêm só os matriculados.

A visualização carrega o HTML num `iframe` dentro do modal, a partir de uma URL
`blob:` — que é de mesma origem da aplicação. É isso que permite o botão *Imprimir*
chamar `contentWindow.print()` e mandar para a impressora só o documento, sem a
interface do sistema em volta. O blob é liberado ao fechar o modal.

Com saída em `.zip`, o formato *Visualizar / Imprimir* não abre o modal: baixa um
zip com um `.html` por aluno.

No modo "arquivo único", cada aluno começa em página nova por meio de uma **quebra de
seção** — é isso que faz o cabeçalho e o rodapé do modelo se repetirem em todos.

---

## 3. Como escrever o modelo no Word

Escreva os códigos entre chaves duplas, exatamente como aparecem na tela de consulta:

```
Declaramos que o(a) aluno(a) {{NOMEALUNO}}, matrícula {{MATRICULA}},
está matriculado(a) na turma {{TURMADESCRICAO}} ({{TURNO}}).
```

Regras:

- **Maiúsculas**, sem acento e sem espaço dentro das chaves: `{{NOMEALUNO}}`
- Códigos funcionam no corpo, em tabelas, no cabeçalho e no rodapé
- Campo sem valor no banco sai vazio — nunca sai `null` nem `{{CAMPO}}`
- Código que não existe no catálogo sai vazio (e o cadastro avisa)

Há um modelo pronto para teste em
`harpia-docs/Modelo_Exemplo_Declaracao_Matricula.docx`.

**Limitação conhecida:** imagens dinâmicas (colar a foto do aluno dentro do documento)
exigem um módulo pago do docxtemplater e não estão incluídas. Imagens fixas do modelo —
logo, brasão, assinatura digitalizada — funcionam normalmente.

---

## 4. Catálogo de campos

Tabela `DocumentosWordCampos`, comum a todas as escolas.

| Coluna | Para quê |
|---|---|
| `Codigo` | o que se escreve no Word, entre chaves duplas |
| `Descricao` | o que aparece na tela de consulta |
| `Grupo` | `ALUNO`, `ACADEMICO`, `FINANCEIRO`, `EMPRESA`, `ESPECIAL` |
| `Origem` | caminho dentro do contexto montado pelo backend (ex.: `aluno.Nome`) |
| `Formato` | `TEXTO`, `DATA`, `DATAEXTENSO`, `MOEDA`, `NUMERO`, `SEXO`, `ESTADOCIVIL`, `TURNO`, `SITUACAO`, `SITUACAOFINAL`, `SIMNAO` |

### Incluir um campo novo

Se a informação vier de uma tabela **já unida** pelo `ContextoService`, basta um
`INSERT` — sem deploy:

```sql
INSERT INTO DocumentosWordCampos (Codigo, Descricao, Grupo, Origem, Formato, Ordem, Ativo)
VALUES ('DATACONCLUSAO','DATA DE CONCLUSAO','ACADEMICO','aluno.DataConclusao','DATA',1360,1);
```

Namespaces disponíveis em `Origem`:

`aluno` (Alunos) · `matricula` (AlunosPeriodo) · `turma` (Turmas) · `nivel`
(NivelSequencia) · `curso` (Cursos) · `professor` (Funcionarios) · `plano`
(PlanosPagamento) · `convenio` (Convenios) · `endereco` / `endfin` / `endaca`
(Enderecos, tipos ALUNO/FIN/ACA) · `respfin` / `respaca` (AlunosResponsavel) ·
`empresa` (Empresas) · `periodo` (Periodos) · `financeiro` (agregados de
Mensalidades) · `calc` (calculados) · `sistema` (data, hora, usuário).

Campo vindo de uma tabela nova (notas, frequência, histórico) exige alterar
`app/Services/DocumentosWord/ContextoService.ts`.

---

## 5. Arquivos da função

### Backend (`path-api`)

```
database/scripts/documentos_word.sql                    tabelas + catálogo + menu
app/Controllers/Http/DocumentosWordController.ts        CRUD, download, validação
app/Controllers/Http/DocumentosWordCamposController.ts  catálogo
app/Controllers/Http/DocumentosWordImpressaoController.ts  alunos + geração
app/Services/DocumentosWord/ContextoService.ts          monta os dados por aluno
app/Services/DocumentosWord/MergeService.ts             docxtemplater + junção + zip
app/Services/DocumentosWord/DocxRenderService.ts        Puppeteer + docx-preview → PDF e HTML
app/Services/DocumentosWord/Formatadores.ts             data, moeda, sexo, turno...
start/routes.ts                                         (bloco adicionado no final)
```

### Frontend (`harpia-ui`)

```
src/pages/DocumentosWord.tsx                            lista de modelos
src/pages/DocumentosWordForm.tsx                        cadastro + upload
src/pages/DocumentosWordCampos.tsx                      consulta de campos
src/pages/Impressao/DocumentosWord_Impressao.tsx        impressão
src/interfaces/IDocumentoWord.ts
src/interfaces/IDocumentoWordCampo.ts
src/utilities/downloadArquivo.ts                        download de binário da API
src/config/Routes.tsx                                   (3 rotas nos dois blocos)
```

### Endpoints

| Método | Rota | O que faz |
|---|---|---|
| GET | `/documentosWord` | lista os modelos (sem o BLOB) |
| POST | `/documentosWord` | cadastra (multipart, campo `Arquivo`) |
| PUT | `/documentosWord` | altera (arquivo opcional) |
| DELETE | `/documentosWord` | exclui |
| GET | `/documentosWord/download` | baixa o modelo original |
| GET | `/documentosWord/validar` | lista os códigos usados e os desconhecidos |
| GET | `/documentosWord/campos` | catálogo (filtros `Localizar` e `Grupo`) |
| GET | `/documentosWord/alunos` | alunos por turma, nome ou matrícula |
| POST | `/documentosWord/gerar` | gera `.docx`, `.pdf`, `.html` ou `.zip` |

O corpo do `gerar` aceita `Saida: 'UNICO' \| 'ZIP'` e
`Formato: 'DOCX' \| 'PDF' \| 'HTML'` — as combinações funcionam todas
(um `.zip` de PDFs, um `.zip` de HTMLs, etc.).

---

## 6. Próxima etapa

Os 4 filtros da tela de impressão ficaram para a 2ª etapa e precisam da regra exata
de cada um:

- Mês de aniversário
- Pendência de documentos
- Pendência financeira
- Situação acadêmica

A tela já está montada de forma a receber esse bloco de filtros sem reorganização.
