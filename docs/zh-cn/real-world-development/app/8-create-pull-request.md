---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在应一并审查这些内容并创建拉取请求。星级评分拉取请求 (PR) 是由你亲自合并的；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会把功能、指令更新、技能更新和 QA 智能体拆分为几个独立的 PR。为简化工作坊流程，你将整个筛选和质量工作流保留在同一会话和分支中，并将所有工作纳入此 PR。

本课将介绍如何：

- 了解 Agent Merge 及其如何自动处理合并流程。
- 检查完整的功能 PR 和验证证据。
- 审查后再授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发人员控制合并授权。

## 认识 Agent Merge

**Agent Merge** 会在 GitHub Copilot app 中自动处理拉取请求合并前剩余的工作。启用后，会话会读取 PR 并处理阻碍事项，包括修复失败的持续集成 (CI) 检查、回应审查意见，以及在需要时变基；GitHub 允许合并后，它会自动完成合并。该功能在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，你一直自行选择 **Merge pull request**。Agent Merge 可以承担这项工作，但它编辑代码和合并 PR 的能力仍需你明确授权。授予合并权限前，请先审查它可执行的操作和相关工作。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查后，让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁边的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅选择下拉菜单选项并不会启动流程。

Agent Merge 随即启动。它会：

- 使用标题和说明创建拉取请求。
- 如果会话从议题启动，则在 PR 正文中引用相关议题。
- 针对目标分支执行变基或处理潜在的合并冲突。
- 监视 CI 流程，确保所有检查通过。
- 监视 PR 中其他开发人员或 Copilot 代码审查提供的反馈，并进行更改以解决相关意见。
- 可选择在所有检查成功后自动合并 PR。

让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁边有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进处理进度并解决报告的阻碍因素。继续之前，确认 PR 状态为 **Merged**；已启用合并权限或智能体仍在运行，都不代表 PR 已经合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护规则或权限限制。继续前先解决这些阻碍因素。

## 总结与后续步骤

你已自动处理开发流程中的多个环节，包括生成、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，你将[使用现有画布并创建分类画布][next-lesson]，体验与智能体共同检查、规划和可视化工作的更丰富方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
