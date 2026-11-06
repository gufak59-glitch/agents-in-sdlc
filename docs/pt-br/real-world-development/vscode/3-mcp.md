---
title: "Exercício 3 - Testar a funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você criou a filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Neste exercício, você conectará o **servidor MCP do Playwright** para que o Copilot controle um navegador real, teste a funcionalidade e, depois, publique sua branch e abra o PR.

Você vai conhecer o Model Context Protocol (MCP), adicionar o Playwright ao workspace e usá-lo para testar a filtragem.

## O que é Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é a forma como o Copilot faz isso: um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento do modo de agente e sua interação com contexto, LLM e ferramentas, incluindo ferramentas de servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Assim, eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

O acesso a ferramentas e recursos acontece por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor gerencia a comunicação entre o agente e ferramentas externas, como APIs existentes ou ferramentas locais — por exemplo, pacotes NPM. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: APIs para administrar repositórios, issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: automação de navegador com Playwright.

Há muitos outros servidores MCP que oferecem acesso a ferramentas diferentes. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente seu código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores em que confia e tenha cautela ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores disponíveis no workspace.

1. Abra `.vscode/mcp.json` no codespace.
2. O servidor remoto `github` já deve estar configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem com o modelo do projeto e permitiu que o Copilot consultasse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não há nada para instalar localmente: o VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Adicione uma segunda entrada para que o Copilot possa controlar um navegador.

1. Em `.vscode/mcp.json`, adicione `playwright` ao lado de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar mais tarde que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparece acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme a confiança no servidor, revise a configuração e escolha confiar nele para permitir que seja iniciado.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar de suas ferramentas.

## Testar a filtragem

O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador real. Em vez de clicar pela aplicação para verificar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma forma rápida de verificar o comportamento de uma funcionalidade sem sair da conversa.

Por baixo dos panos, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. O agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista), como a tecnologia assistiva, então uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar a funcionalidade que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo não parecer correto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se o servidor de desenvolvimento foi parado, inicie-o novamente antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador; se o Copilot informar que falta um, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

1. No codespace, abra o painel **Source Control** e revise as alterações.
2. Selecione **+** para prepará-las e use o botão **Sparkle** para gerar uma mensagem de commit.
3. Selecione **Publish** para enviar a branch.

   ![Painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

## Criar o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, deixe que ele crie o PR: ele pode localizar a issue relevante e associá-la ao PR.

1. No painel Copilot Chat, selecione **New Chat** para iniciar uma nova sessão.
2. No seletor de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não faça merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- conheceu o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar manualmente a funcionalidade em um navegador antes de publicá-la;
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Em seguida, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
