---
title: "练习 9 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选委派给云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 也提供了许多用于交互的斜杠命令。这些命令可以访问高级功能、查看后台信息并提供其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他一些实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

CLI 核心工作流已经完成。现在来看看其他一些功能——管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令检查或手动压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有各自偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员一起工作并分享经验，有助于所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help` 并查看当前安装版本中 `/share` 的选项。
2. 如果要共享本次会话，请选择符合需求的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 将导出的内容发送给他人或发布之前，先进行审查。会话导出内容可能包含提示、回复和项目信息。

是否发布链接或 gist 由你决定。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心研讨会内容已完成。Copilot CLI 还为更大型的任务提供了以下命令：

- `/fleet` 可以将相互独立的子任务分配给子智能体并行运行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体，由其异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程工作。尝试前，请开始一个范围明确的新任务，并通过常规工作流审查结果。想深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 中的斜杠命令可以配置工具、共享会话并了解后台运行情况。在本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！最后，通过[回顾所学内容][next-lesson]并查看后续学习资源，结束这段学习旅程。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
