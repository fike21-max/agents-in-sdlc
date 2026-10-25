---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务问题。这些问题可能包括尚未解决的安全问题、需要更新的旧代码，或因一直没有时间实现而搁置在待办列表中的功能请求。GitHub Copilot 云代理可以自主执行更新代码和添加功能等任务。代理完成工作后，会创建一个草稿 PR，供开发者审查。这样就能把繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本课将通过 Copilot 云代理了解以下内容：

- 自定义代码生成环境。
- 确保操作安全地执行。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初负责创建网站的承包商没有妥善处理文档——事实上，你会发现文档完全缺失。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，也就是同一类别中的其他游戏。暂时不需要精致的实现，只要有可用于验收测试用户体验的版本即可。虽然这项工作目前受阻，但此时还有其他优先级更高的问题。

这两项工作都可能很快被搁置，因此很适合分配给 Copilot 云代理。Copilot 云代理可以异步处理它们，让开发者专注于其他任务，之后再审查 Copilot 的工作并确认一切符合预期。

## 介绍 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发者一样在后台执行任务。与人类开发者协作类似，你可以通过多种方式分配任务，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进展、设置环境并开始处理任务。会话进行期间或完成后，你都可以查看 Copilot 的会话。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你！

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法——不能因此跳过思考。即使看似简单的操作，深入分析后也会发现复杂之处，所以[为 Copilot 云代理设定任务范围时务必谨慎][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已经完成自定义说明实操练习，就已经看到了 `.github/copilot-instructions.md` 的作用；如果还没有，现在正好快速了解一下。在将工作分配给 Copilot 云代理之前，只读查看此存储库中已有的说明文件，以便之后留意这些说明产生的影响。

在存储库的 GitHub Web UI 中，或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些说明限定在 `**/*.test.ts` 文件中。

在下一部分将 **Code lacks documentation** 问题分配给云代理时，请留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和注释标题——这些内容来自上述说明文件。之后的练习中审查 PR 时，我们还会再次指出这一点。

## 设置 Copilot 云代理的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保一切处于正常状态。将任务分配给 Copilot 也一样，因为它的工作方式与软件工程师类似。

云代理使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过创建[专用设置工作流][setup-workflow]自定义此环境，配置文件为 `.github/workflows/copilot-setup-steps.yml`，并在代理开始工作前运行。这可以让代理使用所需的开发工具和依赖项。实验流程已预先配置了这些步骤，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充种子数据的 SQLite 数据库（用于单个 Astro 应用）：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件看起来与其他 GitHub 工作流相同，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这对于测试工作流是否成功很有用，无需等待 Copilot 触发它。

## 添加文档

所有人都知道文档的重要性，但大多数项目中的文档要么已经过时，要么完全缺失。这类技术债务经常得不到处理，降低了工作效率，也让维护代码库或帮助新开发者加入团队变得更加困难。幸运的是，Copilot 很擅长编写文档，因此这非常适合作为分配给 Copilot 云代理的问题。它会在后台生成必要的文档。之后的练习中，你会回来审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建新问题。
5. 将 **Title** 设置为 `代码缺少文档`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在有帮助的情况下为函数和方法添加 TSDoc 文档注释。不幸的是，最近的更新没有遵循这一标准。我们需要更新现有代码，确保在有助于阐明行为时添加文档注释。
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等一两分钟，然后刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中生成待办事项列表。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，应会看到 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 留意你可以滚动查看实时会话，以及 Copilot 如何解决问题。这包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及创建代码。

这可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让我们可以专注于其他工作。我们将同时利用这一优势：再给 Copilot 云代理分配一项任务，然后转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云代理的一大优势是可以拆分工作，让你专注于一组任务的同时，由代理处理另一组任务。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间也可以用来处理其他任务。现在把这项工作分配给 Copilot 云代理！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`在游戏详情页显示相关游戏`
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。需求如下：

   - 在 `src/lib/` 中添加一个数据访问辅助函数，返回指定游戏所属类别中的其他游戏，但不包括该游戏本身
   - 在游戏详情页显示“相关游戏”部分，并使用该辅助函数
   - 处理游戏没有相关游戏的情况
   - 为新的辅助函数创建单元测试
   - 创建 PR 前，确保所有测试通过
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将问题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你应该会在问题的第一条评论中看到一组 👀，表示 Copilot 已开始处理问题！

![Copilot 使用眼睛表情符号表示已开始处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二个请求！Copilot 云代理的工作方式与软件工程师类似，因此你不必一直监控它，等它完成后再审查即可。接下来，我们将学习如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。使用云代理，你可以将问题分配给 Copilot，让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或帮助将代码从一个框架迁移到另一个框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保操作安全地执行。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

Copilot 云代理正在后台工作。接下来，我们将转而创建和使用自定义代理。[Copilot 云代理也可以使用 MCP 服务器][cloud-agent-mcp]，并且能读取自定义说明；前面的模块已经介绍过自定义说明。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
