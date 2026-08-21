# Relatório da Atualização — 21/08/2026

Atualização da documentação funcional do Harpia. Escopo do período: mudanças **visíveis ao
usuário** entregues entre **07/08/2026 e 21/08/2026**.

> Base comparada: última geração da documentação em **07/08/2026** (commit `87cf820`), mais o
> commit de documentos do Word de 18/08. Foram analisados os commits das aplicações
> (`harpia-ui` e `path-api`) posteriores àquela data, filtrando apenas o que **muda a experiência
> do usuário** (telas, campos, botões, mensagens, regras, menus e relatórios). Refatorações,
> ajustes internos, correções de tipagem e reorganização de componentes foram ignorados conforme
> a regra de ouro.

Foi um período atípico: além dos ajustes de rotina no Financeiro, entraram **três funcionalidades
inteiramente novas** (Documentos Word, Dashboard com widgets e assistente virtual) e uma mudança
estrutural de comportamento do **menu**, que agora respeita as permissões do perfil.

---

## Novas páginas criadas

| Página | Conteúdo |
|--------|----------|
| `docs/documentos-word/_category_.json` | Categoria do novo módulo (posição 12.5, entre Tabelas de Secretaria e Administrativo) |
| `docs/documentos-word/README.md` | Visão geral do módulo Documentos Word, conceitos e ordem sugerida de uso |
| `docs/documentos-word/modelos.md` | Tela **Modelos de Documentos** — cadastro e envio do `.docx` |
| `docs/documentos-word/campos.md` | Tela **Campos Disponíveis** — catálogo de códigos, grupos e formatos |
| `docs/documentos-word/impressao.md` | Tela **Impressão** — geração por aluno/turma, saída e formato |
| `docs/getting-started/dashboard.md` | **Dashboard (Meus widgets)** — widgets disponíveis e configuração |
| `docs/getting-started/assistente-virtual.md` | **Talita** — assistente virtual, limites e uso |
| `docs/gerenciador/gerenciador-tabelas.md` | **Gerenciador de Tabelas** — tela técnica restrita a Master |
| `docs/release-notes/relatorio-atualizacao-2026-08-21.md` | Este relatório |

---

## Páginas atualizadas

| Página | O que mudou |
|--------|-------------|
| `docs/intro.md` | Índice geral: nova seção **Documentos Word**, novas páginas em Primeiros Passos e no Gerenciador, e lista de notas de versão corrigida (faltavam agosto/2026 e os relatórios de 31/07 e 07/08) |
| `docs/getting-started/navegacao.md` | Novas seções: **menu filtrado pelas permissões**, **tela inicial (Dashboard)** e **ajuda pela Talita** |
| `docs/gerenciador/perfil.md` | Documentadas as **cinco permissões por tarefa** (Visível, Incluir, Editar, Deletar, Imprimir), a marcação automática entre elas, a marcação em massa pelo cabeçalho e o efeito real no menu lateral. Novos exemplos, FAQ e links relacionados |
| `docs/tabelas-financeiras/cartao.md` | Novo campo **RPU (Recebimento de Parcelamento Único)**: campo, regras, exemplo, FAQ e fluxo |
| `docs/financeiro/recebimento-mensalidade.md` | Marcação **- RPU** no dropdown de cartões e bloqueio do campo **Qtd. Parcelas**; nota sobre o estorno parcelado |
| `docs/financeiro/outros-recebimentos.md` | **Limite de dias para pagamento retroativo**, cartões **RPU** e exibição do número do recibo no estorno |
| `docs/financeiro/ficha-financeira.md` | Novo grupo de colunas **Pagamento** (Desc.+Ext., M + J, Déb.Cor., Dat. Pagto., Pago, Nº Recibo) e **bloqueio de pagamento retroativo** |
| `docs/financeiro/contas-a-pagar.md` | **Pagamento parcelado no cartão institucional de crédito** (campo Qtd. Parcelas, geração de uma conta por parcela) e **prévia do estorno parcelado**, com novo fluxograma |
| `docs/financeiro/recibo.md` | Selo **CORTESIA** no recibo e número do recibo nos estornos |
| `docs/configuracoes/financeiro.md` | Detalhamento do parâmetro **Permitir Pagto./Transf. Fonte p/ Data retroativa** com o campo de **dias** e a tabela de comportamento |
| `docs/relatorios/financeiros.md` | Coluna **Recibo** no Diário Financeiro (Consulta Diário) |
| `docs/release-notes/2026-08.md` | Nova seção **Novidades da segunda quinzena**, com as onze entregas do período |

---

## Páginas removidas

Nenhuma. Nenhuma funcionalidade documentada foi descontinuada no período.

---

## Funcionalidades alteradas (com impacto ao usuário) no período

1. **Documentos Word (novo módulo).** Três telas novas — Modelos, Campos Disponíveis e Impressão.
   Permite preencher automaticamente documentos `.docx` da própria escola com os dados do aluno
   ou da turma, com saída em Word, PDF ou visualização para impressão.
2. **Dashboard com widgets configuráveis.** Cinco painéis disponíveis; cada usuário escolhe quais
   exibir e em que ordem.
3. **Talita — assistente virtual.** Botão flutuante presente em todas as telas; responde a dúvidas
   de uso, reconhece a tela atual, sugere perguntas, pode ser arrastado e tem a posição gravada.
   Só aparece quando o recurso está ativado na instalação.
4. **Menu filtrado por permissões.** O menu lateral passou a exibir somente as tarefas marcadas
   como **Visível** no perfil; grupos sem nenhuma tarefa liberada deixam de aparecer. Usuários
   **Master** continuam vendo o menu completo. O menu é recarregado no login e na troca de
   empresa.
5. **Perfil com cinco permissões por tarefa.** Visível, Incluir, Editar, Deletar e Imprimir, com
   marcação automática entre elas e marcação em massa pelo cabeçalho da coluna.
6. **Cartão RPU.** Novo campo no cadastro de cartões; força o recebimento em parcela única e
   aparece como **- RPU** na descrição dos dropdowns de recebimento.
7. **Contas a Pagar — pagamento parcelado no cartão institucional de crédito.** Novo campo
   **Qtd. Parcelas**; gera uma conta a pagar por parcela, vinculada à conta original.
8. **Contas a Pagar — prévia do estorno parcelado.** Janela "⚠️ Informações do Estorno de
   Pagamento Parcelado", com tabela mostrando o que será reativado e o que será cancelado.
9. **Limite de dias para pagamento retroativo.** Passou a ser efetivamente aplicado na Ficha
   Financeira e em Outros Recebimentos, com mensagens específicas.
10. **Selo CORTESIA no recibo.** Recebimentos sem valor recebido saem identificados.
11. **Ficha Financeira — grupo de colunas Pagamento** e **Diário Financeiro — coluna Recibo**.
12. **Gerenciador de Tabelas.** Tela técnica restrita a usuários Master, sem item de menu.

### Alterações ignoradas (sem impacto perceptível)

- Correções de cálculo e de gravação em Recebimento de Mensalidade, Outros Recebimentos e
  Diário Financeiro que **restauram** o comportamento já documentado.
- Ajustes de layout de colunas, indicadores de carregamento (por exemplo, o overlay em Valores
  Complementares) e mudanças de estilo em boletos.
- Ajustes de logotipo em boleto, mudanças de `config`, versão da aplicação e reorganização de
  rotas.
- Início da integração com o **Asaas**: ainda não há tela nem fluxo visível ao usuário final.

---

## Prints que precisam ser atualizados

### Novos (nunca capturados)

- [ ] **Documentos Word — Modelos:** lista de modelos; janela de cadastro com descrição e arquivo;
      aviso de códigos não reconhecidos; documento no Word mostrando os códigos.
- [ ] **Documentos Word — Campos:** lista com o filtro de grupo aberto; confirmação da cópia.
- [ ] **Documentos Word — Impressão:** tela com modelo e turma selecionados; lista de alunos;
      opções de Saída e Formato; janela de visualização com **Imprimir** e **Baixar HTML**.
- [ ] **Dashboard:** tela com widgets ativos; janela **Configurar widgets do Dashboard**; estado
      vazio com a mensagem de nenhum widget ativo.
- [ ] **Talita:** botão flutuante com o balão de convite; painel de conversa com as perguntas
      sugeridas; resposta na conversa.
- [ ] **Gerenciador de Tabelas:** mensagem de acesso restrito; tela com uma tabela selecionada;
      janela **Editar registro**.
- [ ] **Contas a Pagar:** campo **Qtd. Parcelas** habilitado; janela **Informações do Estorno de
      Pagamento Parcelado**.
- [ ] **Configurações do Financeiro:** aba **Período** com a caixa de data retroativa e o campo
      de dias.

### Recapturar (a tela mudou)

- [ ] **Perfil:** lista de tarefas com as cinco colunas de permissão — o print anterior não
      mostra Visível/Incluir/Editar/Deletar/Imprimir.
- [ ] **Ficha Financeira:** tabela de parcelas com o novo grupo de colunas **Pagamento**.
- [ ] **Cartão:** formulário com a caixa **RPU**.
- [ ] **Menu lateral:** capturar um menu de usuário com perfil restrito, para ilustrar a
      filtragem por permissões.

> Os prints devem ser inseridos **na etapa correspondente** de cada página — os marcadores
> `*(Print sugerido: ...)*` já indicam a posição exata dentro do texto.

---

## Pendências encontradas

1. **Item de menu do Gerenciador de Tabelas desativado.** A tela existe e funciona, mas o item
   de menu está comentado no código. A documentação registra o acesso pelo endereço direto e
   sinaliza a situação. Confirmar se a intenção é liberar o item para usuários Master.
2. **Filtros da tela de Impressão de Documentos ainda não implementados.** Quatro filtros
   previstos — mês de aniversário, pendência de documentos, pendência financeira e situação
   acadêmica — ficaram para uma segunda etapa. Não foram documentados, por não existirem na
   aplicação.
3. **Documento técnico fora do Docusaurus.** O arquivo `harpia-docs/DocumentosWord.md`, na raiz do
   projeto, é um guia **técnico** (instalação, dependências, endpoints, arquivos). Ele não faz
   parte da documentação funcional e não aparece na navegação. Sugerido movê-lo para um
   repositório de documentação técnica ou para uma seção separada, para não confundir usuários.
4. **Integração Asaas em andamento.** Há código inicial no backend e no frontend, mas nenhuma tela
   ou fluxo visível. Documentar quando a funcionalidade estiver disponível ao usuário.
5. **Diferenças de fim de linha no repositório.** Todos os 110 arquivos de `docs/` aparecem como
   modificados no controle de versão apenas por normalização de quebra de linha (CRLF/LF), sem
   mudança de conteúdo. Sugerido configurar `.gitattributes` para evitar ruído nos próximos diffs.

---

## Dúvidas que precisam de validação

1. **Talita — disponibilidade por escola.** A assistente só aparece quando ativada na instalação.
   Isso será um recurso por plano/contrato? A documentação hoje apenas orienta a falar com o
   suporte quando o botão não aparece.
2. **Dashboard — permissões.** Os widgets financeiros aparecem para qualquer usuário que acesse o
   Dashboard, independentemente do perfil? Se houver restrição prevista, a página precisa
   registrá-la.
3. **Cartão RPU — significado oficial.** A sigla foi documentada como "Recebimento de Parcelamento
   Único", conforme o rótulo da tela. Confirmar se é essa a nomenclatura que a escola usa no dia a
   dia.
4. **Contas a Pagar parcelado — limite padrão.** Sem limite configurado no cartão, o sistema aceita
   até 99 parcelas. Confirmar se esse é o comportamento desejado ou se deveria haver um teto menor.
5. **Gerenciador de Tabelas — política de uso.** Confirmar se a escola pode usá-la sem suporte ou
   se o acesso deve permanecer exclusivamente sob orientação técnica, como a documentação
   recomenda.

---

## Sugestões de melhoria para a documentação

1. **Prioridade nos prints do novo módulo.** Documentos Word é a entrega mais visível do período e
   é a única sem nenhuma imagem. Uma sessão de captura resolveria as três telas de uma vez.
2. **Guia de primeiros passos para novos colaboradores.** Com o Dashboard, a Talita e o menu por
   permissões, já há material para uma trilha "seu primeiro dia no Harpia" reunindo as quatro
   páginas de Primeiros Passos.
3. **Página única sobre estornos.** O estorno hoje está espalhado por Recebimento de Mensalidade,
   Outros Recebimentos, Contas a Pagar e Ficha Financeira, e ganhou variações (parcelado, com
   prévia). Uma página central com o conceito e links para cada tela reduziria a repetição.
4. **Glossário financeiro.** Termos como RPU, cortesia, dedução, desconto extra e débito corrigido
   aparecem em várias telas. Um glossário evitaria repetir explicações.
5. **Modelos de documento prontos.** Distribuir junto com a documentação alguns modelos `.docx` de
   exemplo (declaração de matrícula, atestado de frequência) aceleraria a adoção do novo módulo.
   Já existe um exemplo no repositório — vale referenciá-lo na página de Modelos.
6. **`.gitattributes` no repositório de documentação.** Para eliminar o ruído de fim de linha nos
   diffs e facilitar a revisão das próximas atualizações.

---

## Verificação final

| Item | Resultado |
|------|-----------|
| Links quebrados | Nenhum. Todos os links relativos criados apontam para arquivos existentes |
| Imagens inexistentes | Nenhuma imagem referenciada; os prints estão como checklists pendentes |
| Páginas órfãs | Nenhuma. Todas as páginas novas foram incluídas no índice de `intro.md` e são alcançadas pela navegação automática do Docusaurus |
| Títulos duplicados | Nenhum. Cada página tem um único `#` de primeiro nível |
| Funcionalidades sem documentação | Ver **Pendências** (filtros da impressão e integração Asaas, ambos ainda não disponíveis ao usuário) |
| Documentação desatualizada | Corrigida a lista de notas de versão em `intro.md`, que estava dois relatórios atrás |
| Menus alterados | Registrada a filtragem do menu por permissões e a ausência do item **Gerenciador de Tabelas** |
| Inconsistências | Alinhada a regra do campo **Qtd. Parcelas** entre Recebimento de Mensalidade, Outros Recebimentos e Ficha Financeira, agora considerando também o cartão RPU |

---

## Histórico

| Data | Alteração |
|------|-----------|
| 21/08/2026 | Relatório criado. |
