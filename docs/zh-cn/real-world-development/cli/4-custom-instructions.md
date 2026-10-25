---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就要让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你希望编写*什么*代码，还说明代码应如何组织。现在你已经构建了筛选功能，接下来将了解 Copilot 使用的说明、添加文档标准，并将其应用到代码中。

本课将学习如何：

- 了解存储库说明和按路径指定的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

与优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的设计决策，而不是重复代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果函数接受可注入的 `db` 参数，也要记录该参数。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 应保留现有的格式和 lint 指南。

通过使用说明文件，你可以确保 Copilot 了解相关信息，并按照这些实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这是引导它给出更相关建议和代码片段的有效方式。你可以指定首选编码约定、库，甚至希望添加的注释类型。说明既可以适用于整个存储库，也可以针对特定类型的文件。

有两种说明文件：

- `.github/copilot-instructions.md`：每个存储库请求都会发送给 Copilot 的单个说明文件。它应包含与存储库的大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南的文件。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖某种格式之前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

为了帮助你开始，入门项目中已包含一组说明文件。先了解现有内容，再进行更改并观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指南。这些说明适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了用于确定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为可复用的小文件，并指向可参考的示例。

## 更新说明文件以匹配团队指南

现有文件已经是一个良好的起点，但还缺少一项内容。我们将修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 遵循注释规范。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约在第 35 行附近。
2. 将以下内容作为最后一条添加：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在不丢失功能上下文的情况下加载新指南。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指南：

   ```plaintext
   我们刚刚更新了说明和代码指南。请根据这些指南更新你生成的代码。
   ```

4. 输入 `/diff` 并通读更改后的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何获取说明文件中的上下文，并将新标准应用到了功能中。具体来说，你：

- 了解了存储库说明和按路径指定的说明文件如何提供给代理。
- 更新说明文件以确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致的方式运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
