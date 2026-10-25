---
title: "练习 1 - 自定义说明（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解一些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你希望它做*什么*，还了解你希望它*如何*完成任务。

本练习将学习如何：

- 了解项目特定上下文、编码指南和文档标准如何通过存储库自定义说明和按路径指定的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 交互方式不同，你不会在此处运行*前后对比*提示。Copilot 云代理会异步处理 GitHub 问题，因此较难实时并排展示其影响。稍后在本交互方式中审查云代理生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

与优秀的开发团队一样，Tailspin Toys 也有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并呈现现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用说明文件，你可以确保 Copilot 了解相关信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这是引导它给出更相关建议和代码片段的有效方式。你可以指定首选编码约定、库，甚至希望添加的注释类型。说明既可以适用于整个存储库，也可以针对特定类型的文件。

有两种说明文件：

- `.github/copilot-instructions.md`：每个存储库请求都会发送给 Copilot 的单个说明文件。它应包含与项目相关的信息，例如适用于大多数 Copilot 聊天或 CLI 请求的上下文。这些内容可以包括所用技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的说明文件。你可以为特定语言（例如 TypeScript 或 Astro），或者诸如创建 UI 组件、新建单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理会在生成代码时使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 最后，Copilot 还支持其他标准的说明文件格式，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

全面探讨如何创建说明文件超出了本教程的范围。不过，示例项目展示了一种典型方法。总体而言：

- 将 `copilot-instructions.md` 中的内容限制在项目级指南，例如项目构建内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供说明。
- 使用自然语言，保持指南清晰，并提供代码示例说明应如何编写（或避免如何编写）。

创建说明文件没有唯一的正确方法，正如使用 AI 也没有唯一的正确方式。你可以通过不断试验，找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的说明文件。浏览此项目中的说明文件时，你可能会注意到它们覆盖了许多任务类型，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`），相应课程会在需要时介绍具体操作。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含许多说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
## 探索此项目中的自定义说明文件

花些时间阅读此存储库自带的说明文件——其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指南。这些说明适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用文件。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 留意专门用于为此项目创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到页面底部。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分为可复用的小文件，并在生成代码时为 Copilot 提供参考示例。（其中的路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求提供代码内文档。接下来的步骤会添加 TSDoc 文档注释和文件级注释标题的规则。

## 添加新的存储库标准

接下来，我们要编辑一处内容：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释标题。之后，在本交互方式中分配问题时，云代理会读取此规则。

编辑之前，请先创建分支（云代理会读取问题目标分支中的说明，但我们仍会通过提交整洁地保存更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约在第 27 行附近。留意其中记录的项目编码标准——目前还没有关于代码内文档的规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，以要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明，也可以为特定语言、文件类型或任务创建 `*.instructions.md` 文件。项目级文件适合存放全局标准，例如刚添加的文档注释规则。

## 提交、推送并合并说明更新

云代理会读取问题目标分支中的说明。在下一练习中将问题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，云代理才能读取这些说明。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 指向 `main` 的拉取请求并合并。下一练习中处理问题时，云代理就会读取这些说明。

> [!TIP]
> 如果希望在本教程中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了模拟真实项目中的常见操作方式。

## 总结和后续步骤

你了解了 Copilot 如何获取此项目说明文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的存储库级标准。之后几项练习中，你会看到 Copilot 云代理生成的拉取请求如何遵循此标准。

接下来，你将[把第一个问题分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 条建议][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
