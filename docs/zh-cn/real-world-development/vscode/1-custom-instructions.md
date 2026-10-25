---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。你可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，为代码生成提供指导。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传达给 Copilot；
- 在当前指令的指导下，为筛选功能生成第一部分数据层代码（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的全局存储库标准；
- 再次发送相同提示，观察生成的代码如何遵循新标准；
- 将指令更新和筛选功能的数据层代码提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设置的某些标准。Copilot 的输出具有非确定性。本练习的重点是观察更新指令后行为的变化趋势，而不是要求输出逐字完全一致。

## 指令文件

### 场景

Tailspin Toys 作为一家优秀的软件开发公司，有一套开发实践指南和要求，包括：

- 数据层必须始终配有单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应使用 TSDoc 文档注释。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助这项强大的功能，你可以引导 Copilot 提供更贴切的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望使用的注释类型。指令既可以应用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：针对该存储库的每次请求，都会将这个唯一的指令文件发送给 Copilot。此文件应包含项目级信息，即与 Copilot 收到的大多数聊天或 CLI 请求相关的上下文，例如技术栈、项目概览、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建这些文件，例如为特定编程语言（如 TypeScript）或任务（如创建 UI 组件或单元测试）编写指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [支持使用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件，为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，让指导清晰易懂，并提供代码应如何编写（以及不应如何编写）的示例。

创建指令文件没有唯一的正确方式，使用 AI 也同样如此。通过实践，你会逐步找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。查看此项目中的指令文件时，你会发现其中涵盖了多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同使用界面的操作方式各不相同，例如在 VS Code 中选择 **Configure Chat → Generate Agent Instructions**，或在 Copilot CLI 中使用 `/init`。相关课程会在适当时介绍对应的操作。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看此项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心文件 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。与 Copilot 交互时，这些内容都适用。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。你会看到有关 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它会设置一个相对于存储库根目录的 glob，用来确定指令适用于哪些文件。此处，匹配 `**/*.test.ts` 等模式的任何 TypeScript 测试文件都会应用这些指令。
5. 留意此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供示例以供参考。链接路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目的编码标准，但尚未要求添加代码文档注释。在接下来的步骤中，你会补充 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

接下来按照最佳实践为更改创建一个分支。

1. 从上一练习返回 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始新的对话。不同练习使用的模式和模型可能不同；需要时，每个步骤都会说明。

要观察自定义指令的作用，先在当前指令文件生效的情况下发送提示。稍后，你会更新该文件并再次发送相同提示，比较结果。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中代理选择器的屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商返回名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和相应测试。
6. 留意建议的辅助函数是一个类型化函数：它以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这是因为 `.github/instructions/drizzle.instructions.md` 中的数据层约定适用于 `src/lib/*.ts`。
7. 留意建议的代码**没有** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，它也有可能添加文档注释。即使出现这种情况也没关系；本练习的重点仍然是观察更新指令后的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准写入该文件，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节通常靠近第 27 行。留意其中记录了项目的编码标准，但尚未规定代码文档注释，因此刚才生成的辅助函数没有包含文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，为文件添加一段注释，说明文件用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所学，指令文件可以在存储库级别（`.github/copilot-instructions.md`）创建，用于提供全局指导；也可以创建 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目范围的标准，例如刚添加的文档注释规则。

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话上下文。
2. 将焦点放在 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，并为每个发行商返回名称和 ID。应用文件更改。
   ```

4. 留意现在建议的文件开头包含类似下面的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的列表，以及各自的 ID 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 promise，解析为发行商对象数组。
    */
   ```

你刚才引导 Copilot 遵循一项新的项目标准，并将其应用于实际代码；下一练习会在此基础上继续构建。

## 提交指令并推送分支

指令文件和其他资源一样，也需要通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建一个拉取请求，并将其关联到待办事项中的文档标准 issue。你可以手动完成这些操作，也可以让 Copilot 代劳：提示它查找 issue、创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍这种连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，同时说明指令更新和新建发行商辅助函数基础：

   ```
   查找与更新指令文件相关的 issue。从当前分支创建新的 PR，说明该 PR 会关闭此 issue，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 如果 Copilot 提示你允许其代表你执行 GitHub 操作，请查看命令，并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR，并将本地分支切回 `main`：

   ```
   将 PR 合并到 main。然后在本地切回 main，并拉取最新代码以确保代码已更新。
   ```

7. 如果 Copilot 提示你允许其代表你执行 GitHub 操作和运行 shell 命令，请先查看命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了一个拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从此项目的指令文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 发送代码生成提示，观察 Copilot 在现有指令下的输出；
- 在 `.github/copilot-instructions.md` 中添加新的全局存储库标准；
- 再次发送相同提示，观察建议的代码如何遵循新标准；
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习可以继续使用。

接下来，Copilot 将在[代理模式][next-lesson]中运用这些指令，在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优质 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
