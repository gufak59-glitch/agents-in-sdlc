---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou houver informações prévias que o Copilot deva conhecer —, esse contexto precisa estar acessível. Há várias formas de compartilhá-lo; entre as principais estão os [arquivos de instruções][instruction-files], que orientam a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções aplicados a caminhos específicos;
- gerar a primeira parte dos dados de filtragem — um helper de editoras — com as instruções atuais;
- adicionar um padrão para todo o repositório em `.github/copilot-instructions.md`;
- executar o mesmo prompt novamente e observar o código seguir o novo padrão;
- fazer commit das instruções e do helper em `main` para que o Copilot use a orientação atualizada na próxima lição.

> [!CAUTION]
> O código gerado pode divergir dos padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários, a interface deve usar modo escuro e visual moderno, o código deve incluir comentários TSDoc e cada arquivo deve começar com comentários que expliquem sua finalidade.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor o estilo e os requisitos de programação. Elas podem orientar convenções, bibliotecas e comentários, e podem valer para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é enviado ao Copilot em **todas** as solicitações do repositório. Use-o para informações gerais do projeto, como a pilha de tecnologias, uma visão geral do que está sendo criado, boas práticas e orientações globais.
- `.github/instructions/*.instructions.md` pode conter instruções para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, criação de componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para autocompletar nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam os arquivos do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também oferece suporte a outros formatos, como `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado em orientações gerais, como uma descrição do que está sendo criado, a estrutura do projeto e padrões globais de programação.
- Use `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do que o código deve — e não deve — fazer.

Não há uma única maneira certa de criar instruções. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Ao explorar o projeto, você verá arquivos para vários tipos de tarefas, incluindo [alterações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface utilizada indicará quando isso for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções deste projeto

1. Abra `.github/copilot-instructions.md`.
2. Examine o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe também as orientações aninhadas sobre workflows do GitHub Actions. Essas instruções se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore-a. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um glob relativo à raiz do repositório para determinar a quais arquivos as instruções se aplicam. Neste caso, os arquivos TypeScript de teste, por exemplo os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e para arquivos existentes no projeto. Isso permite dividir instruções maiores em arquivos menores e reutilizáveis e indicar exemplos a seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> **Code formatting requirements** documenta os padrões atuais, mas ainda não exige documentação no código. Você adicionará regras para TSDoc e cabeçalhos de arquivo.

## Criar uma branch

1. Volte ao codespace e abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Crie e acesse uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará o que selecionar.

1. Feche as guias do editor abertas em exercícios anteriores para que o Copilot use apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deverá ficar.
3. Na visualização Chat, escolha **Agent** no seletor de agentes para que o Copilot possa aplicar alterações aos arquivos.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica atualizações, possivelmente em vários arquivos, incluindo o helper e seus testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> O Copilot pode adicionar comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a consistência após atualizar as instruções.

## Adicionar um padrão para todo o repositório

1. Reabra `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, próxima à linha 27.
3. Adicione estas linhas após os padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> Use `.github/copilot-instructions.md` para padrões globais e `*.instructions.md` para orientações específicas. A regra de comentários de documentação é válida para todo o projeto.

## Executar novamente o prompt e observar a mudança

1. No Copilot Chat, selecione **New Chat**.
2. Volte a `src/lib/publishers.ts` e envie novamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

3. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

4. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com o ID e o nome de cada uma.
    *
    * @param db - Cliente do banco de dados Drizzle.
    * @returns Uma Promise resolvida com uma matriz de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que servirá de base para a próxima lição.

## Fazer commit das instruções e enviar a branch

1. Abra uma nova janela de terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das alterações de instruções e de todos os arquivos alterados pelo Copilot para a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se o git status mostrar outras alterações de apoio (por exemplo, testes), adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer merge de um pull request

Com a branch publicada, crie um pull request e associe-o à issue sobre o padrão de documentação no backlog. O Copilot pode encontrar a issue, criar um PR para fechá-la e, em seguida, mesclar o PR.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao workspace, então não é preciso configurar nada. Na primeira utilização, o VS Code talvez peça para você entrar no GitHub; siga as instruções para autorizar. Você aprenderá como essa conexão funciona (Model Context Protocol) em uma lição posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para localizar a issue sobre a atualização do arquivo de instruções e criar um PR da branch atual, explicando também que foi adicionada a base do helper de editoras:

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot começar a localizar a issue e criar o PR, revise cada ação do GitHub solicitada e selecione **Allow** quando apropriado.
5. Depois que o PR for criado, peça ao Copilot para mesclá-lo e retornar à branch `main` local, atualizando o código:

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Quando solicitado, revise as ações do GitHub e os comandos de shell que o Copilot pretende executar e selecione **Allow** quando apropriado.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar a resposta com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- fazer commit das alterações de instruções e da base do helper em `main` para que a próxima lição possa usá-las.

Em seguida, use as instruções no [modo de agente][next-lesson] para implementar uma funcionalidade.

## Recursos

- [Arquivos de instruções para personalização do GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
