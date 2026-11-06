---
title: "练习 5 - 自定义并使用 quality-checks 技能"
description: "浏览现有 quality-checks 技能、自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已经手动验证代码是否正常运行，并使用说明文件确保代码符合标准。但测试、lint 和其他持续集成（CI）环节又该如何处理？

对于这类任务，**智能体技能**非常适用！技能可以帮助 Copilot 正确执行这些操作。

在本课中，你将：

- 浏览现有的 `quality-checks` 技能；
- 自定义技能结果的格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告格式，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。核心结构是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含定义技能用途的 frontmatter（含名称和说明）、技能功能概述，以及有关何时应调用技能的指导。文件夹中还可以包含供技能调用时使用的脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要额外的文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目中已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

Tailspin Toys 团队创建了一个名为 `quality-checks` 的技能，用于运行单元测试、lint 和类型检查。现在来查看它。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能目前尚无 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在请 Copilot 运行技能中的三项检查。

1. 返回处于 Interactive 模式的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

我们希望得到更好的报告，清楚说明运行了哪些检查、检查是否成功，以及工具报告的内容。现在更新技能以生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

技能已更新，现在来看看效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用之前的**完全相同的提示**：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与首次运行的报告进行比较。

## 总结和后续步骤

你已自定义并使用现有智能体技能。在本课中，你：

- 浏览了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了技能结果的格式；
- 重新加载并运行了技能。

此更改会与筛选功能一起纳入功能 PR。接下来，[使用 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
