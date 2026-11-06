---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para alterar os cartões de jogos, revisar a mudança no navegador encaminhado e mesclá-la no primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, faça sua primeira alteração. Os jogos já têm avaliações por estrelas nos dados, mas elas ainda não aparecem nos cartões da página inicial. Peça ao agente para exibi-las, revise a mudança e faça merge do primeiro pull request.

Nesta lição, você vai iniciar uma conversa focada em uma branch de funcionalidade, solicitar uma pequena alteração, revisar o diff, executar o aplicativo no navegador encaminhado e criar e mesclar um pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, já exibida na página de detalhes. Os cartões mostram título, categoria, editora e descrição, mas não a avaliação. Como exercício inicial, peça ao agente para exibir a avaliação existente em cada cartão.

## Como funciona uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI. Uma conversa normal usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree. Conversas salvas permitem retomar a discussão; os arquivos e a branch continuam sendo o estado normal do Git no disco.

## Iniciar uma conversa e solicitar a alteração

1. Volte ao codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Inicie o Copilot, se necessário:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com `/new` e pressione <kbd>Enter</kbd>.
5. Envie:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto e criará o código necessário.

## Revisar o diff

1. Digite `/diff` e examine todos os arquivos alterados.
2. Confirme que cada cartão mostra a avaliação numérica ou `No rating yet` quando `starRating` é `null`.
3. Confira se os testes cobrem os dois casos.
4. Revise as verificações executadas pelo Copilot e peça a correção de eventuais falhas.
5. Pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como toda ferramenta de IA generativa, o Copilot é probabilístico. O código pode variar; revise o comportamento, não espere uma implementação idêntica.

## Verificar as alterações

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões exibem as avaliações de zero a cinco estrelas.
4. Peça ao Copilot que pare o servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abrir e mesclar seu primeiro pull request

1. Peça ao agente padrão para fazer commit:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Execute `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e mostrará sua URL.
3. Abra o PR e revise os arquivos alterados e as verificações.
4. Quando estiver tudo pronto, selecione **Merge pull request** e confirme.
5. Volte ao codespace, saia do Copilot CLI com `/exit` e atualize `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Você iniciou uma conversa focada em uma branch, pediu a alteração dos cartões, revisou o diff, verificou o site e criou e mesclou seu primeiro PR.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson].

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
