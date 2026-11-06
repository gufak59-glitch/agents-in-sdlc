---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para explorar a filtragem no navegador. Agora, combine esses recursos em um agente personalizado com um processo claro de garantia de qualidade (QA).

Nesta lição, você explorará como agentes personalizados usam instruções, skills e ferramentas MCP, criará e revisará um perfil reutilizável de QA e o usará para comparar os resultados com a issue de filtragem.

## Cenário

Antes de abrir um pull request, a Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador. Um agente personalizado pode coordenar esse processo e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida por um perfil Markdown que descreve sua finalidade, instruções e ferramentas disponíveis. Neste workshop, você criará `.github/agents/qa.agent.md` e selecionará o agente no Copilot CLI.

As personalizações têm funções diferentes: as instruções registram os padrões da equipe; a skill `quality-checks` reúne verificações repetíveis; o MCP do Playwright fornece ferramentas de navegador; e o perfil de QA orienta o Copilot a usar esses recursos para avaliar requisitos e relatar resultados.

## Criar o perfil de QA

1. Volte ao codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisar o perfil

1. Execute `/diff` e abra `.github/agents/qa.agent.md`.
2. Confira o frontmatter: `description` é obrigatório; `name` é opcional, mas ajuda a identificar o agente.
3. Confirme que o perfil começa pelos requisitos, segue as instruções, executa `quality-checks` e usa Playwright MCP.
4. Verifique se o agente apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se faltar algum requisito ou limite, peça ao agente padrão para revisar o perfil.

## Executar QA para a issue

O Copilot CLI carrega agentes do projeto ao iniciar uma conversa. Retome a conversa de filtragem e selecione QA para usar o contexto da issue e do plano.

1. Ative o agente:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como o agente acabou de ser criado, talvez não apareça na lista. O comando acima ainda o ativa.

2. Peça:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Leia o relatório do agente de QA.

## Resumo e próximos passos

Você combinou instruções, skills e ferramentas MCP em um perfil reutilizável de QA e revisou os resultados da funcionalidade de filtragem.

Agora, a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, [reúna o trabalho em um PR e use Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
