---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar a filtragem e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Alterações maiores exigem um processo mais completo. Nesta lição, você seguirá um fluxo de desenvolvimento com agentes: partirá de uma issue, planejará uma funcionalidade, verificará o código e o comportamento e, depois, fará o merge. Você usará o mesmo contexto e a mesma branch nas próximas lições para atualizar instruções, personalizar a skill `quality-checks`, validar com MCP, criar um agente de QA e abrir o PR.

> [!NOTE]
> Normalmente, você usaria branches ou PRs diferentes para cada tipo de alteração. Neste workshop, o fluxo de filtragem e qualidade compartilha uma conversa e uma branch para manter o foco nos conceitos principais.

Nesta lição, você iniciará uma conversa a partir de uma issue, definirá requisitos em Plan, implementará em Autopilot, revisará o código e testará a funcionalidade no navegador encaminhado.

## Cenário

O catálogo da Tailspin Toys está crescendo e visitantes precisam filtrar jogos por categoria e editora. A issue descreve a funcionalidade, mas decisões como combinar filtros precisam ser definidas antes de programar.

## Contexto

O desenvolvimento com agentes de IA não muda os fundamentos. Um fluxo comum é:

1. Começar com uma issue que descreva o trabalho.
2. Planejar o que será criado.
3. Implementar e revisar o código.
4. Executar testes.
5. Validar manualmente a funcionalidade.
6. Criar um pull request.
7. Após a revisão e a aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes variam entre equipes, mas muitos fluxos seguem essas etapas. Elas ajudam a verificar se o código gerado atende aos requisitos e passa pelas mesmas revisões do código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: o agente sugere alterações e aguarda sua orientação.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente escreve código, executa testes e itera de forma autônoma.

Comece em Plan, revise o plano e depois use Autopilot para implementá-lo.

## Começar pela issue

1. Volte ao codespace e confirme que o PR de avaliações por estrelas foi mesclado.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

4. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

5. Pressione <kbd>Tab</kbd> duas vezes para abrir **Issues**, pressione <kbd>A</kbd> para exibir todas as issues e realce **Allow users to filter games by category and publisher**.
6. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**. A referência, como `#7`, adiciona a issue ou o PR do GitHub ao contexto.

## Planejar a filtragem

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> até o indicador mostrar **Plan**.
2. Depois da referência da issue, envie:

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. Responda às perguntas do Copilot sobre o comportamento dos filtros.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Revise o plano e peça alterações se ele não refletir suas decisões.

## Aprovar o Autopilot

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente.
2. Confirme que o indicador mostra **Autopilot**.
3. Acompanhe a implementação e a execução dos testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; revise o plano primeiro. Se o Copilot informar dependências ausentes ou conflito de porta, resolva a configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para voltar ao modo Interactive.
2. Digite `/diff` e examine a implementação e os testes em comparação com a issue e suas decisões.
3. Pressione <kbd>Esc</kbd> para sair do diff e revise os resultados das verificações.

## Explorar a nova funcionalidade

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando a porta `4321` estiver disponível no Codespaces, selecione **Open in Browser**.
3. Teste os filtros de categoria e editora, suas combinações, a redefinição e os resultados vazios.
4. Peça ao Copilot que pare o servidor.

## Resumo e próximos passos

Você iniciou uma conversa a partir de uma issue, definiu requisitos em Plan, implementou com Autopilot, revisou o código e verificou a funcionalidade no navegador.

Em seguida, use [instruções personalizadas][next-lesson] para que o código siga as práticas documentadas.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
