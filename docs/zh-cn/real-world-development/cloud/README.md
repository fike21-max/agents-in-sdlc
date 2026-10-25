---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)**让 GitHub Copilot 在云端异步工作。你在 GitHub 上分配任务，云代理便会在后台接手——探索存储库、进行更改并打开拉取请求——与此同时，你可以自由处理其他工作。

在这些练习中，你将添加云代理要遵循的自定义说明，然后分配一个 GitHub 问题并让它实现任务。你将审查并使用自定义代理来影响它的处理方式，从代理仪表板监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云代理要遵循的自定义说明 |
| [2. 云代理][ex2] | 异步代理 | 将问题分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本教程之前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education]，经过验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本教程所用的代理、MCP、代码审查和 Copilot CLI 功能，因此你可以使用该计划完成所有交互方式的教程。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云代理，只有 Copilot Free 计划除外。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本交互方式不介绍 MCP。想了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 交互方式](../cli/)或 [VS Code 交互方式](../vscode/)。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
