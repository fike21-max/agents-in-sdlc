---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，整合问题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，你将把这些能力整合到一个自定义代理中，并为其定义清晰的 QA 流程。

本课将学习如何：

- 了解自定义代理如何与说明、技能和 MCP 工具协同工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选问题审查其发现。

## 场景

在打开拉取请求（PR）之前，Tailspin Toys 希望对需求、代码质量、自动化检查、测试覆盖率和浏览器行为进行一致的审查。自定义代理可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述代理的用途、说明和可用工具。在本教程中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何运用这些能力评估需求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 之前，请让 Copilot 创建可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查以及必须遵守的界限。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请求默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建一个名为 QA 的自定义代理。它应根据问题和达成一致的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求报告为通过、失败或受阻，并提供支持证据。它必须在更改实现代码前征求同意，并且不得提交更改或打开拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新代理前，请审查其配置文件，确认 Copilot 捕捉到了预期的 QA 工作流和约束。这样可以避免代理在你只想验证功能时做出不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需项；`name` 是可选项，但填写后代理会有清晰的显示名称。
3. 阅读配置文件说明，确认 QA 从需求开始，遵循存储库说明，运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在更改实现代码前征求同意，并且不会提交更改或打开拉取请求。
5. 如果生成的配置文件遗漏了任何职责或约束，请要求默认代理修改后再继续。

## 根据问题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一个筛选对话并选择 QA，以便它使用已加入上下文的问题和规划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 你刚创建的代理可能不会出现在可用代理列表中。它仍然存在，可以通过上述命令启用。

2. 使用以下提示，请 QA 代理审查功能：

   ```plaintext
   根据问题和计划中作出的决定审查筛选功能。它适合提交 PR 了吗？
   ```

3. QA 代理会开始工作！
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义代理如何与说明、技能和 MCP 工具协同工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选问题审查了它的发现。

现在，你已准备好实现、技能更新、QA 配置文件、测试和验证报告，可以开始审查。接下来，你将[整合这些内容到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
