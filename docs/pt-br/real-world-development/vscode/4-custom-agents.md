---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` fornecem instruções, boas práticas, padrões de código e conhecimento específico. As equipes podem codificar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem as diretrizes [WCAG][wcag], de segurança ou de testes.

Os agentes são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt que define o comportamento e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em arquivos Markdown. Uma distinção prática: o **agente personalizado** executa e coordena o trabalho; as **skills** são ferramentas. Agentes têm sua própria janela de contexto e podem orquestrar skills ou outros agentes. Neste laboratório, o agente de acessibilidade revisará o site e poderá usar skills de pull request ou de testes.

> [!NOTE]
> Não há uma única maneira certa de escrever um agente. Teste e itere para encontrar o que funciona no seu ambiente.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma acessível para todas as pessoas. Comentários de usuários indicam que o tema escuro é difícil de ler por causa do contraste insuficiente. A equipe de design solicitou um modo de alto contraste com uma alternância para ativá-lo ou desativá-lo. Você usará um agente personalizado para implementar a funcionalidade.

## Revisar o agente personalizado de acessibilidade

Crie uma branch separada da `main` para o trabalho de acessibilidade:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Revise as responsabilidades, as boas práticas de acessibilidade, os exemplos de HTML, CSS e JavaScript e as armadilhas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados de `.github/agents` no seletor de agentes na parte inferior do Copilot Chat.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa.

1. No seletor de agentes, escolha **Agent**, se ainda não estiver selecionado.
2. Abra o seletor de agentes e escolha **Accessibility agent**.
3. Envie:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. O Copilot explorará o código, proporá alterações e editará arquivos. Revise os diffs exibidos na conversa e acompanhe **Files changed**.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot pode alterar componentes Astro, CSS e testes relacionados. Você acompanhará e orientará o trabalho na próxima lição.

## Resumo e próximos passos

Você revisou o agente personalizado de acessibilidade e o usou no modo de agente do Copilot Chat para implementar o modo de alto contraste.

Em seguida, [monitore e oriente o trabalho do agente][next-lesson], incluindo um pedido para adicionar um modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar a funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
