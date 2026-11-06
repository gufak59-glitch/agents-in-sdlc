---
title: "Lição 5 - Personalizar e usar uma skill de quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver também envolve testes, lint e outras partes da integração contínua (CI). Para essas tarefas, **skills de agente** são uma boa opção: elas ajudam o Copilot a entender como executar operações repetíveis.

Nesta lição, você explorará a skill `quality-checks`, personalizará o formato dos resultados, recarregará e executará a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

Skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos de apoio que o agente carrega sob demanda. Em geral, uma skill é uma pasta com um arquivo Markdown `SKILL.md`, que contém frontmatter com `name` e `description`, uma visão geral e orientações sobre quando usá-la. A pasta também pode incluir scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais são opcionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md` porque usa comandos já existentes no projeto.

Skills podem ficar em `.github/skills` para serem compartilhadas pela equipe ou na pasta pessoal `~/.copilot/skills`.

## Explorar a skill

1. No codespace, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description`; a descrição ajuda o Copilot a decidir quando usar a skill.
3. Examine as instruções para executar testes e lint.
4. Observe que ainda não há uma seção **Results output formatting**.

## Executar a skill antes da alteração

1. Volte à conversa de filtragem no modo Interactive.
2. Peça:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Leia o relatório.

## Personalizar o relatório

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione ao final:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Execute:

   ```plaintext
   /skills reload
   ```

2. Envie novamente o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Leia o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você explorou a skill `quality-checks`, personalizou o formato de resultados, recarregou a skill e a executou. A alteração acompanhará a filtragem no PR.

Em seguida, use o [MCP do Playwright][next-lesson] para permitir que o Copilot interaja diretamente com o site.

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
