---
title: "Lição 10 - Revisão e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo na Tailspin Toys. Você preparou o Codespace, instalou a CLI, explorou o projeto e encontrou a issue de filtragem; adicionou avaliações por estrelas e mesclou o primeiro pull request; planejou, implementou e revisou a filtragem usando Plan, Autopilot e Interactive; atualizou instruções, personalizou `quality-checks`, testou com Playwright MCP, criou um agente de QA, revisou a alteração e autorizou o Agent Merge. Também explorou comandos de barra para contexto, modelos, compartilhamento e delegação.

## O que você entregou

O workshop inclui dois PRs, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas**: exibir o `starRating` existente e um estado explícito para jogos sem avaliação.
2. **Filtragem e qualidade**: implementar filtros, atualizar e aplicar instruções, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Da etapa de planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código com testes automatizados, uma inspeção manual no navegador e a exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos. O agente de QA reuniu os resultados, os requisitos e a cobertura antes da abertura do PR.

Testes novos devem cobrir lacunas reais; uma execução de QA sem novos testes também pode ser correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após alterações.

## Boas práticas

O contexto e as ferramentas influenciam o trabalho do Copilot. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem papéis especializados e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** conforme a tarefa: use **Plan** para definir uma abordagem, **Interactive** para mudanças focadas e **Autopilot** para tarefas bem delimitadas. Use um modelo rápido para edições rotineiras e outro mais capaz para trabalhos complexos. Descreva claramente *o que* deseja criar, *por que* e *como*.

## Mais recursos para explorar

- `/review` para pedir uma análise ao agente de revisão de código.
- `/rubber-duck` para discutir um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

Continue usando a ferramenta em código de produção, projetos pessoais e ideias que queira desenvolver. Compartilhe o que aprender com a equipe e consulte a documentação.

Para ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar ambientes, explore os workshops do [VS Code][vscode], do [aplicativo GitHub Copilot][app] ou do [agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
