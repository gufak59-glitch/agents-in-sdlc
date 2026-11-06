---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] dizem ao Copilot não apenas *o que* fazer, mas também *como* estruturar o código. Agora que você criou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você examinará instruções do repositório e com escopo por caminho, atualizará o arquivo de instruções para registrar os padrões de programação e observará o impacto no código.

## Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: comentários devem explicar a intenção, funções exportadas em `db/` e `src/lib/` devem ter comentários TSDoc/JSDoc com finalidade, parâmetros e retornos (incluindo o argumento injetável `db`, quando aplicável), componentes Astro reutilizáveis devem documentar o contrato `Props` e os comentários devem continuar atualizados. Preserve também as orientações existentes de formatação e lint.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências sobre convenções, bibliotecas e comentários.

- `.github/copilot-instructions.md` é enviado ao Copilot em **todas** as solicitações do repositório e contém informações gerais.
- `.github/instructions/*.instructions.md` fornece orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a formatos de instrução varia entre ambientes. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar as instruções personalizadas deste projeto

1. No codespace, abra `.github/copilot-instructions.md` e observe a descrição e as orientações gerais do projeto.
2. Explore `.github/instructions`, que contém arquivos para Astro, a camada de dados Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`, que define os arquivos aos quais as instruções se aplicam.
4. Leia `.github/instructions/drizzle.instructions.md` e os links para outros arquivos de instruções e exemplos do projeto.

## Atualizar os arquivos de instruções

Adicione uma diretriz para comentários TSDoc no arquivo principal.

1. Em `.github/copilot-instructions.md`, localize **Code formatting guidance**, próxima à linha 35.
2. Adicione como último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar a orientação atualizada

O Copilot CLI carrega as instruções do repositório ao iniciar uma conversa. Retome a conversa de filtragem para usar a nova orientação sem perder o contexto.

1. Se a conversa ainda estiver aberta, execute `/exit`.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa:

   ```bash
   copilot --resume
   ```

3. Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
4. Peça:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. Execute `/diff`, examine os arquivos TypeScript e confirme que os comentários TSDoc descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI recebe contexto dos arquivos de instruções e aplicou uma nova diretriz à funcionalidade. Em seguida, [personalize e execute a skill reutilizável `quality-checks`][next-lesson] para executar verificações de forma consistente.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
