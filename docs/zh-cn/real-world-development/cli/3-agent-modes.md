---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 就实现方式达成一致，使用 Autopilot 根据问题实现筛选，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先从给项目添加一个小功能开始。但较大的更改需要更严谨的流程。好在 GitHub Copilot CLI 专为融入组织现有流程而设计，帮助我们以正确的方式构建正确的功能。本课是系列课程的第一课，你将遵循典型的代理驱动开发流程：先使用问题生成新功能，再确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 在继续完成此功能工作流时，你会使用同一个对话和分支。通常，不同文件类型的工作会放在不同分支或 PR 中，但本教程会简化这一点，让我们专注于核心概念。

本课将学习如何：

- 从 GitHub 问题开始新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成此功能时，你还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的问题描述了这项功能，但组合类别等细节需要在编码前达成一致。你将使用 Plan 模式解决这些决策，然后授权 Autopilot 在明确的范围内进行实现。

## 背景

将 AI 编码代理引入开发流程，并不会改变软件开发的基本原则。事实上，这些原则变得更加重要！大多数开发者遵循的流程大致如下：

1. 从已提交的问题开始，该问题描述了要完成的工作。
2. 制定要构建内容的计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码符合需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试，并不断迭代，直到认为任务已完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从问题开始

开始筛选功能的工作之前，请返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可以将 GitHub 上的问题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你有机会在交给 Copilot 实现之前，先确定功能或任务的实施方法。处理复杂工作时，花些时间规划通常是个好主意。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用之后，输入以下提示：

   ```plaintext
   制定实现此功能的计划。
   ```

Copilot 会开始制定计划！它会先探索项目，然后确定最佳实现方法。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答即可，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以审查计划。
6. 如果计划中的某些部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现了！现在使用 Autopilot，让 Copilot 开始工作。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或在你安装的版本中选择类似名称的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，必须像其他代码一样，在合并前进行审查。让我们一起审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与问题及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

好的，代码看起来不错——但它能运行吗？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及计划中商定的组合筛选方式。
4. 确认重置和无结果时的行为符合问题描述和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了一项功能。本课中，你：

- 从 GitHub 问题开始新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，我们将[使用自定义说明][next-lesson]深入了解如何生成代码，确保代码符合已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
