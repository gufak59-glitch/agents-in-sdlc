---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções, boas práticas, padrões de código e conhecimento específico para orientar o Copilot. Assim, equipes podem transformar sua experiência em agentes reutilizáveis — por exemplo, para acessibilidade, segurança ou testes.

Os agentes são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills] têm algo em comum: ambos costumam ser definidos em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa e coordena o trabalho, enquanto as **skills** são ferramentas que podem ser usadas durante esse trabalho.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills de fluxo de pull request ou de testes.

> [!NOTE]
> Não há uma única maneira certa de escrever um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com agentes personalizados, você vai explorar:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários de usuários indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre texto e fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é essencial, você usará um agente personalizado para gerar essa funcionalidade rapidamente.

## Revisar o agente personalizado de acessibilidade

Um agente de acessibilidade já foi criado. Revise seu conteúdo para entender como ele orienta o Copilot.

1. No codespace, abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` do frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que descrevem:
   - as responsabilidades ao gerar código para um site acessível;
   - boas práticas de acessibilidade;
   - exemplos de HTML, CSS e JavaScript;
   - erros comuns a evitar.

## Criar e atribuir uma issue

O painel de agentes é o local central para atribuir tarefas ao agente de nuvem, monitorar o trabalho e fornecer orientações adicionais. Atribua ao Copilot uma tarefa para criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. No campo **Title**, insira `Add high contrast mode to website`.
5. No campo **Description**, insira:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecione **Create**.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
8. Selecione **Accessibility agent** na lista de agentes personalizados.
9. Selecione **Assign**. O Copilot começará a trabalhar em segundo plano.

   ![Captura de tela da atribuição do agente de nuvem, com as opções de agente personalizado e acessibilidade destacadas](../../../_images/ex5-select-custom-agent.png)

## Resumo e próximos passos

Você explorou os agentes personalizados do GitHub Copilot, assistentes especializados que ajudam a orientar tarefas e domínios específicos. Também revisou o agente de acessibilidade e atribuiu a ele uma issue.

Agora, você pode [monitorar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
