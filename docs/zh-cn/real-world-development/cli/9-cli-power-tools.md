---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以使用高级功能、查看幕后信息并提供其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他一些实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云代理的可选命令。

## 场景

你已完成 CLI 核心工作流。现在来看看其他一些功能：管理上下文、切换模型、共享会话，以及选择将工作委派给 [Copilot 云代理][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理规模较大或更复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 使用量，以及上下文如何分配给系统说明、工具、消息和剩余空间。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 当上下文窗口接近满载时，Copilot CLI 会自动压缩上下文。想自行选择压缩时机时，请使用 `/compact`。切换到无关任务并希望开启新对话时，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发者也有不同偏好。Copilot CLI 可以列出并选择你想使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和使用信息。
3. 保留当前模型、选择另一个模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果想共享此会话，请选择符合需求的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 将导出的内容发送给任何人或发布之前，先进行审查。会话导出内容可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算公开的存储库或对话内容。

## 可选：扩展并行处理或委派工作

核心教程已完成。Copilot CLI 还提供了用于处理较大型任务的命令：

- `/fleet` 可以将相互独立的子任务分配给子代理，并行运行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云代理，由它异步工作并可能打开拉取请求。

这些命令是可选的，因为它们可能会创建额外的工作树或远程工作。尝试之前，请先开始一个范围明确的新任务，并通过常规工作流审查结果。如果想深入了解异步代理工作，请继续学习[云代理教程][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 中的斜杠命令，可以配置 CLI、共享会话并了解幕后运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云代理的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！让我们通过[回顾所学内容][next-lesson]和后续学习建议，为这段旅程画上句号。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
