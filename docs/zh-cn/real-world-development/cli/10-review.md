---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续开发工作流。你：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建功能，并在 Interactive 模式下审查；
- 使用自定义说明引导智能体，随后自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能运行结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 处理筛选功能 PR；
- 探索用于上下文、模型、共享和可选委派到云端的斜杠命令。

## 交付的功能

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能、更新说明并将说明应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为了简化研讨会流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求审查和测试覆盖率检查整合到一起，再交付 PR。

新增测试应解决实际缺口；如果 QA 检查后认为不需要新测试，结果也可能正确。授权合并前，请审查代码和证据；更改代码后，应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义内容，并根据团队需求调整。说明用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专家角色，而 MCP 服务器用于连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 在开始构建前思考方案；使用 **Interactive** 参与小范围更改；对于范围明确的任务，使用 **Autopilot**。常规编辑可选速度较快的模型，复杂工作则可选能力更强的模型。

上下文仍然和基础设施同样重要。清晰描述要构建的内容、原因和方式，会显著影响输出结果。

## 继续探索

核心工作流已经完成。还可以了解 Copilot CLI 的其他功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是继续使用它！可以用它编写生产代码、兴趣项目，或多年一直想做却尚未开始的小应用。与团队分享经验，也向团队成员学习。最后，别忘了继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
