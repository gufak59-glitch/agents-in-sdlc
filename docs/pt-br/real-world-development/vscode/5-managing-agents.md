---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot explora o código, propõe alterações, edita arquivos e executa comandos. O Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições em tempo real. Você pode revisar cada diff, aceitar ou rejeitar alterações e orientar a conversa com novos prompts.

Neste exercício, você vai monitorar o trabalho do agente de acessibilidade, revisar as edições e orientá-lo para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade da [Lição 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design também solicitou um modo claro. Em vez de começar uma nova conversa, você orientará a sessão existente.

## Monitorar o progresso do Copilot

1. Volte à visualização Copilot Chat da lição anterior.
2. Acompanhe as chamadas de ferramentas, os arquivos lidos e as edições propostas.
3. Para cada diff exibido na conversa, selecione **Keep** para aceitar ou **Undo** para reverter.
4. Abra **Files changed** no início do chat para conferir todos os arquivos alterados.

> [!NOTE]
> Se uma chamada pedir permissão — por exemplo, para executar um comando no terminal — o agente aguardará sua confirmação. Aprove somente comandos que você considera seguros.

5. Aguarde a conclusão do modo de alto contraste e leia o resumo do agente.

## Orientar o agente para adicionar o modo claro

Como a conversa continua, o agente mantém o contexto das alterações, dos componentes e da estratégia de armazenamento local.

1. Envie o prompt:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente analisará as alterações anteriores e proporá mudanças para o modo claro.
3. Revise cada diff e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, selecione **Stop** ao lado do campo de chat e envie um esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Peça ajustes específicos, como simplificar o CSS com variáveis, integrar o modo claro ao painel existente ou adicionar atalhos de teclado.

## Revisar o conjunto de alterações

Antes do commit, revise o trabalho:

1. Abra **Source Control** no VS Code.
2. Examine a lista de arquivos alterados e os diffs.
3. Confirme o uso de atributos ARIA, navegação por teclado, HTML semântico e armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações. Você as publicará na próxima lição.

## Resumo e próximos passos

Você monitorou e orientou uma sessão em andamento, revisou as edições conforme eram propostas, aceitou ou rejeitou alterações e enviou um prompt de acompanhamento para ampliar o trabalho sem perder contexto.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
