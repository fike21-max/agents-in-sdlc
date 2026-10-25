---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你要求 Copilot 为代码添加文档、为设计团队构建相关游戏功能以供迭代，并实现包含高对比度和浅色模式切换的无障碍功能。现在来检查代码更改；如有必要，还可以向 Copilot 提供反馈，改进其工作。

### 场景

正如前面多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用审查之前，我们先检查 GitHub Copilot 为代码添加文档、实现相关游戏功能和无障碍功能的建议。

## 安全性和 GitHub Copilot 云代理

由于 Copilot 云代理会异步且自主地执行任务，因此设置了一些安全限制以确保操作安全。这些限制包括：

- Copilot 只能读取存储库，并且只能写入它用于生成代码的分支。
- 云代理在 GitHub Actions 内运行，并会创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须先由人工批准才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云代理生成的第一个拉取请求（PR），即为代码添加文档的 PR。你将使用 GitHub.com 上的标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **添加缺失的文档** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作。这可能需要几分钟，你可以借此休息一下，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。实际更改可能有所不同。

    浏览更改时，请查看是否添加了 TSDoc 文档注释、是否遵循 TypeScript 约定，以及是否添加了注释标题。这些内容来自练习 2 开始时审查的自定义说明文件。

6. 完成更新审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。完成可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像在拉取请求中提及其他团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新会话来处理它们。由于结果具有非确定性，我们无法给出规定性的提示文本。可以要求 Copilot 完成以下更新：

- 在每个代码文件顶部添加注释标题，简要说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 添加评论，请求对生成的文档进行更改，并像提及用户一样提及 **@copilot**。可以采用上述任一建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 如何执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，你应该会在拉取请求中看到新的提交。
5. 选择 **Files changed** 选项卡以审查更改。

可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转为准备审查状态，然后合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决“在游戏详情页显示相关游戏”问题生成的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **在游戏详情页显示相关游戏** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 完成更新审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准的指示器。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。完成可能需要几分钟。
9. **可选：**你还可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，你可以通过迭代不断改进 Copilot 云代理的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或者确认已添加注释标题和 TSDoc 文档注释（请记住，在更新自定义说明**之前**就已经分配了此任务！）。和之前一样，你可以在 **Conversation** 选项卡中添加新评论来提出这些要求，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的无障碍功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心请求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **为网站添加高对比度模式** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会包含 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意以下方面：
   - 用于切换模式的切换 UI 组件
   - 用于保存用户偏好的本地存储
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 完成更新审查并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单个 Astro 应用的项目检查应该会通过。完成可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认它们正常工作且页面重新加载后设置仍然保留。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进空间，可以像之前处理其他 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云代理最适合处理真实待办事项，让你可以转而专注其他工作。若想养成这种习惯，可以针对存储库创建更多问题并分配给 Copilot。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理交互方式的实验。在这些课程中，你：

- **检查了此存储库自带的自定义说明文件**，以了解它们对云代理输出的影响。
- **将问题分配给 Copilot 云代理**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用了无障碍自定义代理**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作任务控制中心**，监控并引导无障碍会话。
- **审查并迭代了云代理的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 复盘和后续步骤

你已完成云代理交互方式的实验。如果想继续探索，其他交互方式可以补充本次实践的内容：

- 🖥️ **[VS Code 交互方式](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 交互方式](../../cli/)** — 在终端中交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

在自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率问题分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
