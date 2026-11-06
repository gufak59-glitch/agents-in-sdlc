---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体专为更新代码和添加功能等任务而设计，可以自主执行这些工作。智能体完成任务后，会创建一个待审查的草稿 PR。这样既能将繁琐任务交给智能体，加快开发进程，也能让开发者专注于更重要的工作。

你将在本练习中探索如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有妥善编写文档——事实上，代码完全缺少文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页中显示相关游戏，也就是相同类别的其他游戏，以便访客继续浏览。暂时不需要完善的实现，只要能用于用户体验验收测试即可。目前这项工作被其他高优先级议题挡在待办列表中。

这类任务很容易被不断推迟，正适合交给 Copilot 云智能体异步处理。开发者可以先完成其他工作，之后再回来审查 Copilot 的成果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建一个草稿拉取请求来跟踪进度、设置环境并开始处理任务。在会话运行期间或完成后，你都可以查看 Copilot 的操作。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 没有魔法，使用它并不能省去思考过程。即使看似简单的操作，也可能包含复杂环节，因此[为 Copilot 云智能体明确任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段工作、不断学习、尝试和调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的引导作用。如果你已完成自定义说明的练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体之前，先以只读方式浏览存储库中的说明文件，之后就能留意它们的影响。

在 GitHub Web UI 中打开以下文件，也可以在已启动的 codespace 中查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定为适用于 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述说明文件。之后的练习还会在审查 PR 时再次讨论这些内容。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保项目状态正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样完成一系列工作。

云智能体使用 [GitHub Actions][github-actions]环境执行任务。你可以创建一个[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在智能体开始工作前运行，以便安装所需的开发工具和依赖项。实验室已预先配置此工作流，方便你学习。它会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器，并为这个单一 Astro 应用迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可以从存储库的“Actions”选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤授予所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流类似，但有几个关键点：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 请注意 `workflow_dispatch` 触发器。它让你可以从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 运行前验证工作流是否成功。

## 添加文档

尽管大家都知道文档的重要性，许多项目仍然存在信息过时或完全缺少文档的问题。这类技术债务常常无人处理，会拖慢工作效率，并增加维护代码库或帮助新开发者加入团队的难度。Copilot 很擅长生成文档，因此很适合通过云智能体处理这项议题。它会在后台生成所需的文档，你将在后续练习中回来审查成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。这段时间里，Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这通常需要几分钟。Copilot 云智能体的主要优势之一是能够异步执行任务，让你专注于其他工作。接下来会利用这一点：先给云智能体分配另一个任务，然后转而为应用编写新功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是可以并行安排工作，你可以专注于一组任务，而让智能体处理另一组任务。为设计团队添加相关游戏区域可能不会花太久，但这段时间仍可用于处理其他工作。现在就把它分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，议题的第一条评论旁边就会出现 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！云智能体的工作方式类似软件工程师，你不必一直监控它，可以等它完成后再审查结果。现在继续了解自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理任务。它可以帮助解决技术债务、开发新功能，或协助将代码迁移到其他框架。

本课探索了以下内容：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 明确议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始了解如何创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并支持自定义说明；前面的模块已经介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
