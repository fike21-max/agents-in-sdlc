---
title: "第 2 课 - 添加星级评分：快速入门"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以对项目进行第一次更改了。先从一个小改动开始：游戏数据中已经有星级评分，但主页上的游戏卡片还没有显示它。你将请求代理显示评分、审查更改，并将它作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上开始一次有明确范围的 Copilot 对话。
- 请求代理对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，并且游戏详情页已显示评分。不过，主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让代理在每张卡片上显示已有的评分。这是一项小巧且范围明确的更改，非常适合第一次使用 Copilot。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作空间。与 Copilot 应用不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，你会看到三类内容：你的提示和代理的回复；代理探索和编辑文件时的工具活动；以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始一个新对话，实现这项功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动了新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情的文件，并创建所需代码。你已经使用 Copilot CLI 添加了一项新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使是很小的改动也不例外。现在就在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在评分存在时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 与所有生成式 AI 工具一样，结果是概率性的，而非确定性的，因此你看到的代码可能有所不同。请审查功能行为，而不是期待某种固定实现。

## 检查更改

当然，我们不能只读代码并假定它能正常工作。让我们请 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的界面。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并请求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

你已经创建了这项功能！现在创建一个拉取请求（PR），将新代码合并到项目中。

1. 请求默认代理提交更改：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 确认无误后，选择 **合并拉取请求**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上开始了一次有明确范围的 Copilot 对话。
- 指示代理对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选问题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
