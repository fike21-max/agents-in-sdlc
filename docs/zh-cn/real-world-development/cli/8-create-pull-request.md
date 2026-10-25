---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置文件和测试都保存在同一个分支上。现在应一起审查这些内容并打开拉取请求。上一次，你亲自合并了星级评分拉取请求（PR）；这次将由 **Agent Merge** 管理此流程。

> [!NOTE]
> 通常，我们会将功能、说明更新、技能更新和 QA 代理拆分到几个不同的 PR 中。为了简化本教程，你在同一个对话和分支中完成了整个筛选及质量工作流，并将所有更改放在这个 PR 中。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查之后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时由开发者掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理 PR 被阻止的原因——修复失败的持续集成（CI）检查、回应审查意见，以及在需要时进行变基——然后在存储库允许时启用 GitHub 自动合并。

到目前为止，你一直是自己选择 **合并拉取请求**。Agent Merge 可以接手这项工作。启用 Agent Merge 之前，请先审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码全部完成后，让我们一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监控 PR 上的持续集成流程。一切成功后，它会执行合并。
7. 确认看到 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请阅读报告的原因，并在存储库允许时自行完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查之后启用 Agent Merge，并确认 PR 已合并。

接下来，你将[探索其他实用的 Copilot CLI 斜杠命令][next-lesson]，用于管理上下文、模型、会话共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
