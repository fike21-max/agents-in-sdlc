---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你要求 Copilot 云代理完成了三项任务，重点是改进用户体验并添加功能。虽然云代理可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。你可以在这个任务控制中心查看所有有未关闭拉取请求（PR）的代理任务，了解已执行的操作，还可以引导进行中的会话。

本课将学习如何：

- 浏览代理页面，监控云代理任务。
- 引导正在运行的会话，请求添加功能。

## 场景

将高对比度模式任务分配给代理后，团队发现也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，将这项功能一并加入似乎合乎逻辑。你希望引导代理的工作，确保它不仅添加高对比度模式，还添加浅色模式。

## 查看 Copilot 云代理任务

了解所有分配给 Copilot 云代理的任务当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行过的操作。操作可能包括：
    - 从存储库检出代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成指定任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会根据 Copilot 的工作状态和采用的方法而有所不同。

4. 同时留意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和已更改文件，以便进一步监控。

## 引导云代理

现在你已了解当前正在运行的任务，可以请求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择指向添加高对比度模式任务的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。开始看到类似以下内容的消息时，就说明它已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。还应为此模式提供一个切换开关，让用户可以选择所需的显示模式。
    ```

    ![代理页面中的云代理任务截图，其中高亮显示 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会开始处理更新后的任务！它会在收到消息时正在执行的特定步骤完成后，将新请求纳入工作流程。

和之前一样，这需要几分钟时间。你可以借此机会暂停一下，回顾目前所学和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控并引导 GitHub Copilot 云代理任务的中心控制台。你可以在此跟踪所有活动任务和已完成任务、审查正在执行的工作，甚至重新引导正在运行的任务，以调整范围或提供其他指导。

你了解了以下概念：

- 浏览任务控制中心和代理页面，监控云代理任务。
- 重新引导正在运行的会话，请求添加功能。

Copilot 正在完成无障碍功能。接下来，我们将学习如何[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
