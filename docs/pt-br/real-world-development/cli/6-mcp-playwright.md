---
title: "Lição 6 - Validar a funcionalidade com MCP do Playwright"
description: "Configure ou revise o MCP do Playwright no Copilot CLI e use-o para testar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Além de escrever código, é preciso trabalhar com dados, serviços externos e automações. Servidores MCP ampliam o Copilot CLI com ferramentas e serviços adicionais.

Nesta lição, você conhecerá o Model Context Protocol (MCP), adicionará o servidor MCP do Playwright se ainda não estiver disponível e pedirá ao agente que use um navegador para testar a filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. O Playwright MCP permite que o Copilot use o site como uma pessoa usuária e aumente a confiança de que as alterações funcionam.

## O que é Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem em tempo real com ferramentas e serviços externos. Um servidor MCP faz a ponte entre o agente e essas ferramentas. Entre os servidores conhecidos estão:

- **[GitHub MCP Server][github-mcp]**: APIs para gerenciar repositórios, issues e pull requests.
- **[Playwright MCP Server][playwright-mcp-server]**: automação de navegador com Playwright.

O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores.

> [!CAUTION]
> Trate servidores MCP como qualquer dependência. Revise o código-fonte, verifique quem publicou o servidor e considere os riscos antes de permitir acesso a recursos ou operações sensíveis.

## Adicionar o servidor MCP do Playwright

Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas.

1. No codespace, abra o Copilot CLI e execute:

   ```plaintext
   /mcp add
   ```

2. Informe `playwright` como nome, escolha o tipo de servidor **STDIO** e, no campo **Command**, insira:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

3. Salve com <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) e pressione <kbd>Esc</kbd> para fechar a caixa de diálogo.

## Pedir ao Copilot para explorar a funcionalidade com Playwright

Peça ao Copilot que valide a funcionalidade sem alterar o código:

```plaintext
Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
```

O Copilot normalmente identifica sozinho o servidor, mas mencioná-lo ajuda a orientar a tarefa. Acompanhe as etapas no navegador, leia o relatório e confira se os comportamentos correspondem ao esperado.

## Resumo e próximos passos

Você aprendeu o que é MCP, adicionou o servidor Playwright quando necessário e usou o agente para explorar a filtragem em um navegador real.

Em seguida, [crie um agente personalizado de QA][next-lesson] que combine skills e ferramentas de navegador.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
