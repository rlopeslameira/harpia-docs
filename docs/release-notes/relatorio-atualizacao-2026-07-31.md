# Relatório da Atualização — 31/07/2026

Atualização semanal da documentação funcional do Harpia. Escopo do período: mudanças **visíveis
ao usuário** entregues em **31/07/2026**, concentradas no **módulo Financeiro**.

> Base comparada: última geração da documentação em 30/07/2026. Foram analisados os commits das
> aplicações (`harpia-ui` e `path-api`) posteriores àquela data, filtrando apenas o que **muda a
> experiência do usuário** (telas, campos, botões, mensagens, regras e relatórios). Refatorações,
> ajustes de tipos e mudanças internas foram ignorados conforme a regra de ouro.

## Novas páginas criadas

- `docs/release-notes/relatorio-atualizacao-2026-07-31.md` — este relatório.

Nenhuma nova página de funcionalidade foi necessária: as mudanças do período incidiram sobre
telas já documentadas.

## Páginas atualizadas

- `docs/financeiro/recebimento-mensalidade.md` — Qtd. Parcelas por cartão; recibo com parcelas
  (Nx), N° Doc. e Desconto Extra; novas mensagens de validação.
- `docs/financeiro/outros-recebimentos.md` — Qtd. Parcelas por cartão; recibo com parcelas (Nx)
  e N° Doc.; regras e mensagens.
- `docs/financeiro/ficha-financeira.md` — ações por parcela (pagar / imprimir recibo / estornar);
  dados do banco no detalhe da parcela; mensagem de estorno.
- `docs/financeiro/contas-a-pagar.md` — conta sugerida pela última usada no credor; ícones C/D de
  cartão institucional na coluna Fatura; indicador de carregamento.
- `docs/financeiro/geracao-boletos.md` — modelo de carnê para bancos com layout 60.
- `docs/relatorios/financeiros.md` — botão Exportar para Excel no Diário Financeiro e totais.
- `docs/release-notes/2026-07.md` — seção "Atualizações de 31/07/2026".

## Páginas removidas

- Nenhuma.

## Funcionalidades alteradas (com impacto ao usuário) no período

- **Recebimento de Mensalidade / Outros Recebimentos:** campo **Qtd. Parcelas** agora só é
  habilitado para **cartão parcelado**, com mínimo 1 e teto no limite do cartão; **dinheiro/PIX**
  ficam em 1. Novas mensagens: *"O número de parcelas deve ser no mínimo 1."* e *"Quantidade
  máxima de parcelas permitida para este cartão é X"*.
- **Recibo (mensalidade e outros recebimentos):** **forma de pagamento com nº de parcelas (Nx)**,
  novo **N° Doc.**, **Desconto Extra** no canhoto e valor pago em destaque.
- **Ficha Financeira:** botões **por parcela** (pagar, imprimir recibo, estornar) com tooltips;
  **dados do banco** no detalhe da parcela; ajustes de largura de colunas.
- **Contas a Pagar:** **conta sugerida** pela última conta usada no credor; **ícones C/D** para
  cartão institucional de crédito/débito na coluna Fatura; **loading** ao gravar/pagar/estornar/excluir.
- **Geração de Boletos:** novo **modelo de carnê** (bancos com layout 60), com composição própria
  do Nosso Número; seleção automática do layout conforme o banco.
- **Relatório Diário Financeiro:** **Exportar para Excel** reposicionado para o topo da lista;
  rótulos de tarefas/ordenação abreviados e cartões de totais reorganizados.

## Prints que precisam ser atualizados / capturados

Nenhum print havia sido capturado até aqui (a base é textual). Novas capturas prioritárias:

1. **Recebimento de Mensalidade** — campo Qtd. Parcelas desabilitado (dinheiro) e habilitado (cartão); recibo com "(3x)" e N° Doc.
2. **Ficha Financeira** — coluna de ações por parcela (pagar/recibo/estornar) e detalhe com dados do banco.
3. **Contas a Pagar** — coluna Fatura com ícones **C** (verde/vermelho) e **D** (azul); conta preenchida ao escolher o credor.
4. **Geração de Boletos** — visualização no **modelo de carnê** (banco layout 60).
5. **Outros Recebimentos** — recibo com parcelas (Nx) e N° Doc.
6. **Diário Financeiro** — botão Exportar para Excel no topo da lista.

Salvar as imagens em `docs/assets/images/` e inseri-las na etapa correspondente de cada página.

## Pendências encontradas

- **Screenshots** continuam inexistentes; todas as páginas mantêm marcadores (checklists) indicando onde inserir.
- **Bancos com layout 60:** confirmar quais bancos/convênios usam esse layout para citar exemplos concretos na doc.
- **Rótulos de menu por escola:** os caminhos seguem as rotas do sistema; rótulos exatos dependem do menu configurado por tenant.

## Dúvidas que precisam de validação

- **Permissões por tela:** confirmar quem pode **estornar** um pagamento pela Ficha Financeira (a ação passou a ficar acessível na própria linha da parcela).
- **N° Doc. no recibo:** validar em quais formas de pagamento o número de documento deve sempre aparecer.
- **Modelo de carnê:** confirmar se o carnê substitui totalmente o boleto avulso para os bancos layout 60 ou coexiste.

## Sugestões de melhoria para a documentação

- Criar um **guia de captura de prints** (resolução/tema/nomenclatura) para padronizar as imagens pendentes.
- Adicionar um **glossário financeiro** (Nosso Número, layout de banco, carnê, cartão institucional C/D).
- Documentar a tela de **consulta/relatório do Diário Financeiro** com mais profundidade (filtros, export).
- Iniciar a documentação dos módulos **não-financeiros** (Alunos, Turmas, Secretaria, Acadêmico, Boletim), ainda sem cobertura funcional.

## Verificação final

- ✓ Links relativos entre documentos: **0 quebrados** (verificação automática nas páginas editadas e em toda a base).
- ✓ Blocos de admonição (`:::note` / `:::tip`) balanceados nas páginas alteradas.
- ✓ Índice/histórico de cada página atualizado com a data 31/07/2026.
- ✓ Nenhuma página órfã criada; o relatório entra na categoria **release-notes** já existente.
- ⚠ Imagens: marcadores presentes, arquivos ainda não capturados (pendência acima).

---

_Gerado automaticamente pela tarefa semanal de atualização de documentação em 31/07/2026._
