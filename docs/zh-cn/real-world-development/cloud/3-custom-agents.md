---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是一种专用 AI 助手，可根据开发工作流中的特定任务或领域进行定制。你可以通过存储库中的 `.github/agents` 文件夹内的 Markdown 文件定义代理，为 Copilot 提供专门的说明、最佳实践、编码模式和领域知识，从而帮助它更有效地执行特定类型的工作。团队可以将专业知识整理到可复用的代理中，例如负责执行 [WCAG][wcag] 合规要求的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少有 `name` 和 `description` 字段，后面是定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能的比较

自定义代理与[代理技能][agent-skills-concept]之间有一些合理的重叠。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**负责执行工作，而**技能**是工具。

自定义代理拥有自己的上下文窗口，旨在协调技能（甚至其他代理）完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一正确的方法。与 AI 相关的其他事项一样，应针对自己的环境和场景进行测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将通过自定义代理学习以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的限制。近期用户反馈指出，由于文字与背景颜色对比度不足，一些用户难以阅读当前深色主题。为了解决无障碍问题，设计团队要求实现一种可由用户切换的高对比度模式。

由于无障碍至关重要，你希望尽快实现此功能。你将使用自定义代理生成该功能。

## 审查无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。先审查其内容，了解它会如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接着浏览后续部分，其中介绍：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见问题和错误列表。

## 创建并分配问题

任务控制中心是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot 云代理、监控任务，甚至重新引导正在进行的会话。现在将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建新问题。
5. 将 **Title** 设置为 `为网站添加高对比度模式`。
6. 将 **Description** 设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。用户应能通过切换开关启用或关闭高对比度模式。设置应保存在浏览器的本地存储中。
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配界面的截图，其中高亮显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的专用 AI 助手。通过自定义代理，你可以将团队的专业知识和标准整理到可复用的代理中，从而引导 Copilot 更有效地完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，我们将转到任务控制中心，[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做好准备][org-custom-agents]
- [为企业使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
