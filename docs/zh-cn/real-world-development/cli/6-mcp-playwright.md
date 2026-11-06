---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是编写代码。我们还需要处理数据、外部服务，并让 Copilot 能够使用更多自动化工具。这就是 MCP 服务器的作用。MCP 服务器让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

在本课中，你将：

- 了解什么是模型上下文协议（MCP），以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 请智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，还需要实际与 UI 交互。你希望 Copilot 像用户一样使用正在开发的网站，进一步自动化更改验证流程，并提高对更新符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体与外部工具和服务通信提供了一种方式。借助 MCP，AI 智能体可以实时访问外部工具和服务，获取最新信息并代表你执行操作。

MCP 工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁，每个服务器代表智能体可访问的一组工具和资源。

一些常用的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助大家发现 MCP 生态系统中的项目并参与贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样审查 MCP 服务器。使用前，请检查其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 体验功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 中配置的 Playwright MCP 服务器重复验证。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确告诉 Copilot 使用 MCP 服务器，因为它往往会自行判断。不过，既然知道应该使用哪个工具，明确指出也很有帮助！这样可以提高结果的一致性，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的各项操作，以确认功能是否正常。
3. 阅读报告并确认所有行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，然后向你提供报告。

## 总结和后续步骤

恭喜你通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验了功能！回顾一下，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加了 Playwright MCP 服务器；
- 请智能体驱动浏览器并体验筛选功能。

下一步，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
