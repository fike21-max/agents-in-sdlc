---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行各种操作，例如运行测试。作为开发者，你通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……事情往往会接连不断。

这正是 Copilot 代理模式发挥作用的地方。

Copilot 代理模式专为在 IDE 中更自主地工作而设计。它的工作方式类似开发者：先探索现有项目结构，再进行必要的更新、运行测试等任务，并自动修复发现的问题。下面来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但代理模式和云代理适用于两种不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互；云代理则像异步协作的团队成员，通过 issue 和拉取请求与你配合。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并协调完成更改；
- GitHub Copilot 代理模式如何在 UI 和数据层中实现新功能；
- 合并到代码库之前，如何检查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选游戏。上一练习中，你已经添加了发行商辅助函数；现在将使用 Copilot 代理模式完成数据层、UI 和测试中的其余工作。

## 运行 Tailspin Toys 网站

开始修改之前，先浏览 Tailspin Toys 网站，了解它当前提供的功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，可供用户列出游戏并查看游戏详情。此网站是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；项目没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中，交互功能则通过作用范围较小、使用标准 DOM API 的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，可以启动唯一的 Astro 开发服务器，让网站更容易运行。请按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 在 codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示一个包含 URL 的提示，类似下面的内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示你在浏览器窗口中打开 `http://localhost:4321`，请选中 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从终端选择 localhost URL 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的专用隧道；你的网站服务器现已在其中运行！

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，以及每款游戏的标题、图片和说明。
- **游戏详情页**：选择某款游戏后，会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习使用的模式和模型可能不同；需要时，每个步骤都会说明。

网站的初始实现已经可用，但我们希望增加新功能。先查看待办事项。使用模板设置项目时，系统会自动为你创建一组 issue；请让 GitHub Copilot 显示这些 issue。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 代理可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并非在推荐某个模型。制作本工作坊时使用了 Claude Sonnet 4.5，因此教程中也采用该模型，希望代码建议相对一致，从而带来良好的体验。不过，大语言模型具有非确定性，你可能会发现收到的建议与教程中描述的不同。这完全正常，也在预期之内。

> [!NOTE]
> 由于大语言模型具有非确定性，Copilot 可能会使用其他 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项中的 issue：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项，并根据哪些事项最能帮助用户来协助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

注意，Copilot 还根据它认为对用户最有用的程度为事项排了优先级。

## 查看指令文件

启动代理生成代码之前，先查看将用于向 Copilot 提供工作上下文的指令文件。这里将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含向网站添加功能时的处理指南。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中有关添加功能的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和可访问性原则；
   - 指向各种文件类型专属指令文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件既可以引用其他指令文件，也可以引用项目文件。路径相对于指令文件所在位置。这样既可以拆分较大的指令集、将其整理成较小且易于维护的文件，也可以提供示例和模板供 Copilot 参考。

## 实现筛选功能

完成筛选功能至少需要对应用进行以下三项更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑；
- 为筛选行为添加或更新测试；
- 更新游戏列表页面，添加筛选 UI。

此外，在将所有更改合并到代码库之前，还需要运行测试并确保通过。Copilot 代理模式可以为你完成这些工作！现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，将 UI 指令文件添加为上下文。

   ![显示如何选择 UI 指令文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图代理下拉列表中仍选中了 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中相关 issue 的要求实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub issue 的要求，更新网站，以便按发行商和类别筛选游戏。上一练习已经添加了发行商辅助函数；完成其余数据层、UI 和测试工作时，请保留并酌情完善该实现。完成前确保所有测试都通过。服务器已经在运行，无需重新启动。
   ```

6. 观察 Copilot 如何先探索项目，并找到与所需功能相关的文件。你应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![显示 Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。工作期间，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮；工作进行时无需选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理过程中可能会暂停，也可能看到测试暂时失败。这没关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未发现错误。

   ![显示 Copilot 代理模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要检查 Copilot 或任何 AI 工具生成的代码。

10. 返回运行着网站的浏览器，试用新功能！
11. 确认功能正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 代理模式如何在 UI 和数据层中实现新功能；
- Copilot 代理模式如何探索项目、识别相关文件并协调完成更改；
- 合并到代码库之前，如何检查 Copilot 代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增长，就需要启用分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示和代码时，可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件；
- 考虑如何实现分页：允许用户选择每页大小，还是将其设为固定值；
- 编写提示时提供必要的指导，让 Copilot 按照你的预期实现功能；
- 可能需要与 GitHub Copilot 迭代沟通、要求修改并提供上下文。这是使用 Copilot 时的正常工作方式！

## 资源

- [Copilot 的询问、编辑和代理模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
