---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver tarefas com pull requests abertos, explorar as ações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem;
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar alternância de modos, você orientará o agente para incluir ambos.

## Revisar as tarefas do agente de nuvem do Copilot

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas nos painéis principal e lateral, incluindo a documentação do código, as APIs para alterar jogos e o modo de alto contraste.
3. Selecione uma tarefa em andamento e revise as etapas realizadas, que podem incluir obter o código, preparar o ambiente, configurar servidores MCP e executar o trabalho.

> [!NOTE]
> As etapas exibidas variam conforme o estado da tarefa e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Peça ao Copilot para incluir a alternância para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a configuração termine e o agente comece a trabalhar.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem na página de agentes, com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento e inclui o pedido no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará o pedido atualizado depois de concluir a etapa em andamento. Como antes, isso levará alguns minutos; aproveite o tempo para revisar o que aprendeu.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do agente de nuvem. Você acompanhou as tarefas e redirecionou uma sessão em andamento para solicitar outra funcionalidade.

Agora, [itere sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
