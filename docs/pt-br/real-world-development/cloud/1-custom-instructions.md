---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] explicam não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho;
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da CLI, aqui você não executará um prompt antes e depois da alteração. O agente de nuvem trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado. Você verá a influência do arquivo de instruções ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar modo escuro e ter um visual moderno.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Os arquivos de instruções ajudam a garantir que o Copilot siga essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos de programação. Você pode especificar convenções, bibliotecas e comentários preferidos, criando instruções para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um arquivo enviado ao Copilot em **todas** as solicitações do repositório. Inclua nele informações gerais do projeto, como a stack, o que está sendo desenvolvido e as boas práticas globais.
- `.github/instructions/*.instructions.md`: arquivos para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para autocompletar nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos de instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também oferece suporte a outros formatos, como `AGENTS.md` e `CLAUDE.md`; consulte [o suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como sua descrição, estrutura e padrões globais.
- Use `*.instructions.md` para tipos de arquivo ou tarefas específicos.
- Escreva em linguagem natural e clara e forneça exemplos do que fazer e evitar.

Não há uma única maneira certa de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Um projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste repositório, há exemplos para [alterações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. No VS Code, por exemplo, use **Configure Chat → Generate Agent Instructions**; no Copilot CLI, use `/init`. Para obter modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

O repositório inclui um `copilot-instructions.md` principal e vários arquivos `*.instructions.md`. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Leia a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**.
3. Explore a pasta `.github/instructions`, que contém instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório. Por exemplo, ele pode incluir arquivos TypeScript que correspondam a `**/*.test.ts`.
5. Leia as instruções para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos do projeto. Esses links permitem dividir instruções extensas em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um padrão para todo o repositório

Esta é a única edição feita neste exercício: incluir no projeto uma regra para comentários TSDoc e cabeçalhos de comentário.

1. No terminal do codespace, crie e acesse uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md` novamente e localize a seção **Code formatting requirements**, próxima à linha 27.
3. Logo abaixo dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> Use `.github/copilot-instructions.md` para padrões válidos em todo o projeto e `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas.

## Fazer commit, push e merge da atualização

O agente de nuvem lê as instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, faça merge das alterações de instruções em `main`.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge.

> [!TIP]
> Se preferir trabalhar diretamente em `main`, você pode ignorar a etapa da branch e fazer o commit nela. A branch é usada para refletir um fluxo de trabalho real.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções e adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente produzir.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
