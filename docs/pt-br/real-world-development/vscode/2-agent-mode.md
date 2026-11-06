---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo a atualização mais simples de uma aplicação costuma exigir alterações em vários arquivos e operações como executar testes. O fluxo de trabalho normalmente inclui localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou faltando, fazer outra alteração e assim por diante.

É nesse tipo de tarefa que o modo de agente do Copilot ajuda.

O modo de agente foi desenvolvido para agir com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros que encontrar. Vamos explorar como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> O modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, interagindo por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações;
- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados;
- revisar alterações e testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que o catálogo da Tailspin Toys cresce, as pessoas precisam filtrar jogos por categoria e editora. Na lição anterior, você adicionou um helper de editoras. Agora, conclua a camada de dados, a interface e os testes usando o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site. Ele é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. É uma aplicação Astro que gera páginas HTML estáticas. As páginas consultam um banco de dados SQLite local com Drizzle ORM; não há API de back-end separada nem framework de interface no cliente. Helpers de acesso a dados reutilizáveis ficam em `src/lib/`, e a interatividade usa scripts Astro pequenos com APIs DOM padrão.

### Iniciar o site

1. Volte ao codespace. Você deverá estar na branch `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, será exibido um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo perguntar se deseja abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) enquanto seleciona o endereço do servidor Astro `http://localhost:4321` no terminal. Em um codespace, selecionar esse link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explorar o site

- **Página inicial**: lista jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: mostra informações adicionais, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> No codespace, abra o Copilot Chat e selecione **New Chat**. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

1. No seletor de agentes da visualização Chat, escolha **Agent**. Esse agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e invoca ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> A menção a um modelo não representa preferência dos autores. Ele foi usado para criar o workshop e ajudar a manter os resultados consistentes. Como os LLMs são probabilísticos, suas sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> O Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot para mostrar o backlog de issues enviando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de solicitar a geração de código, examine `.github/instructions/ui.instructions.md`, que fornece contexto sobre como adicionar funcionalidades ao site.

1. No codespace, navegue até `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções, o que permite reutilizar orientações, dividi-las em partes menores e mais gerenciáveis e apontar para exemplos e modelos.

## Implementar a filtragem

A conclusão da filtragem exige pelo menos três alterações separadas na aplicação:

- adicionar ou refinar a lógica de filtragem na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e passar antes de mesclar as alterações à base de código. O modo de agente do Copilot pode executar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui**.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** e **Claude Sonnet 4.5** estão selecionados.
4. Envie:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Observe o Copilot começar explorando o projeto e localizando os arquivos relacionados à funcionalidade. Ele deverá encontrar helpers da camada de dados, a interface e os testes; em seguida, começará a modificar arquivos e executar testes.

   ![Copilot explora os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho está em andamento.

6. Quando solicitado, selecione **Continue** para executar os testes. O Copilot pode precisar iterar entre geração de código e testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

7. É possível que haja pausas e que alguns testes falhem durante o processo. Isso é normal: o Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

8. Explore o código gerado para verificar se há problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer ferramenta de IA.

9. Volte ao navegador com o site em execução e explore a nova funcionalidade.
10. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados;
- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações;
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Em seguida, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson].

### Exercício extra — implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Usando o que aprendeu neste exercício, peça ao Copilot para atualizar o site e implementar a paginação. Considere:

- seguir as boas práticas existentes, inclusive usar os arquivos de instruções;
- decidir se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo;
- fornecer ao Copilot as orientações necessárias no prompt;
- iterar, solicitando alterações e oferecendo contexto — esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
