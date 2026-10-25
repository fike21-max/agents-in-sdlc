---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可以创建针对开发工作流中特定任务或领域进行定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，向 Copilot 提供重点明确的指令、最佳实践、编码模式和领域知识，指导它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的代理，例如确保符合 [WCAG][wcag] 标准的可访问性代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局目录 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后面则是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能的区别

自定义代理与[代理技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行工作的人员，技能是执行工作的工具。**

自定义代理拥有自己的上下文窗口，并且可以在工作过程中调用技能，甚至调用其他代理来协同完成任务。在本实验中，可访问性自定义代理会根据可访问性指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能等。

> [!NOTE]
> 编写自定义代理没有唯一正确的方式。与 AI 的其他应用一样，需要通过测试和迭代找到适合自己环境和场景的方案。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文本和背景颜色之间的对比度不足，一些用户觉得当前深色主题难以阅读。为解决这一可访问性问题，设计团队要求实现可由用户切换的高对比度模式。

可访问性至关重要，因此你希望尽快完成此功能。接下来将使用自定义代理生成此功能。

在本练习中，你将：

- 查看现有的可访问性自定义代理；
- 在 Copilot Chat 中使用可访问性代理实现高对比度模式。

## 查看可访问性自定义代理

已经为你创建了一个可访问性自定义代理。先查看其内容，了解它如何指导 Copilot。

返回 codespace，打开终端并从 `main` 创建一个新分支，以便单独处理可访问性相关工作（练习 3 的筛选 PR 将保留在另一分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接着浏览后续章节，查看其中列出的内容：
   - 为可访问网站生成代码时的核心职责；
   - 可访问性最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Chat 视图底部的代理下拉列表中显示 `.github/agents` 目录下定义的所有自定义代理。你可以选择一个自定义代理，让整个聊天会话遵循该代理的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习使用的模式和模型可能不同；需要时，每个步骤都会说明。

1. 如果 Chat 视图的代理下拉列表中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中会显示当前代理；默认值为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向可访问性代理发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应提供一个高对比度切换开关，供用户启用或关闭；该设置应使用浏览器的本地存储，在页面重新加载后继续保留。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议，并将编辑应用到项目文件中。每项编辑都会以内联方式显示在聊天中，其中包含文件路径和可供检查的差异。
6. 代理修改的工作集会显示在 **Files changed** 指示器中，并随着编辑而更新。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，例如编辑 Astro 组件、CSS 和相关测试。

下一练习将介绍如何查看并引导这一过程中正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，这类代理是针对特定任务和领域定制的 AI 助手。借助自定义代理，你可以将团队的专业知识和标准整理成可复用的代理，指导 Copilot 更有效地完成特定类型的工作。

本练习介绍了以下概念：

- 自定义代理的定义方式；
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理的工作][next-lesson]：查看更改过程，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
