---
slug: zh-cn/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code 中的 **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** 将 GitHub Copilot 带入你日常使用的代码编辑器。在 Visual Studio Code（以及 GitHub Codespaces）中，你将在代理模式下使用 Copilot Chat，通过 MCP 连接外部工具，并使用自定义智能体——全程无需离开 IDE。在 IDE 中，Copilot 可以全面查看文件、终端和问题。

首先添加自定义指令，观察 Copilot 如何遵循这些指令，然后使用代理模式构建一项涵盖 UI、数据层和测试的筛选功能。接下来连接 Playwright MCP 服务器，让 Copilot 操作浏览器，在创建拉取请求之前测试功能。最后，你将审查并使用自定义智能体处理无障碍工作，然后监控、引导并迭代 Copilot 的更改——全程无需离开编辑器。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 在 VS Code 中添加并验证自定义指令 |
| [2. 代理模式][ex2] | 代码生成 | 使用代理模式构建筛选功能 |
| [3. 结合 Playwright 使用 MCP][ex3] | 外部工具 | 使用 Playwright MCP 服务器在浏览器中测试功能 |
| [4. 自定义智能体][ex4] | 专用智能体 | 审查并使用自定义智能体 |
| [5. 管理智能体][ex5] | 监控 | 监控并引导智能体会话 |
| [6. 迭代][ex6] | 审查 | 在本地审查 Copilot 的工作并选择后续步骤 |
| [可选：集成 Foundry][foundry-toolkit] | AI 智能体 | 通过三个模块，使用 VS Code 和 Foundry Toolkit 准备模型、部署智能体并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 有权使用 GitHub Codespaces

> [!TIP]
> 没有付费计划？通过验证的学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
