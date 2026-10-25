---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚才使用代理模式构建了筛选功能。创建拉取请求之前，应先在浏览器中确认功能是否正常。无需亲自逐项操作应用进行检查；你可以连接 **Playwright MCP 服务器**，让 Copilot 在真实浏览器中测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加 Playwright MCP 服务器；
- 请 Copilot 使用该服务器，在浏览器中手动测试筛选功能；
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式可以访问编辑器之外的内容时，其能力会大幅增强。Copilot 通过模型上下文协议（MCP）实现这一点：MCP 是一种标准协议，可让代理与外部工具和服务通信。

![代理模式内部工作原理以及它如何与上下文、大语言模型和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息（使用资源）并代表你执行操作（使用工具）。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理两者之间的通信。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 代理可以创建新存储库、更新现有存储库，以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：借助 Playwright 提供浏览器自动化功能，让 AI 代理可以打开网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同的工具和资源。GitHub 还托管了 [MCP 注册表](https://github.com/mcp)，以便更轻松地发现生态系统中的服务器并参与贡献。

> [!CAUTION]
> 在安全方面，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用 MCP 服务器之前，请仔细检查其源代码、验证发布者并考虑相关安全影响。只使用可信的 MCP 服务器；授予其访问敏感资源或执行敏感操作的权限时务必谨慎。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 条目由项目模板提供，因此 Copilot 能在上一练习中读取待办事项里的 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时让你通过 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供一个可控制的浏览器，正好可用于测试你的功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器。在没有桌面环境可显示窗口的 codespace 中，必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要某个服务器的工具时，VS Code 才会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动服务器是可选操作。如果跳过这一步，VS Code 会在下一步中 Copilot 首次需要使用服务器工具时启动服务器，并提示你确认是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了一个可操作的真实浏览器。代理可以打开页面、导航、应用筛选并读取结果，再向你总结观察到的内容；无需离开对话，就能快速确认功能是否符合预期。

在底层，Playwright MCP 服务器使用页面的[可访问性树][playwright-mcp-server]而不是屏幕截图。这意味着代理会像辅助技术一样，根据结构化且带有标签的元素（按钮、链接、列表项）进行分析。因此，简单的功能检查也能顺便快速检查基本可访问性。

连接服务器并启动应用后，向 Copilot 发送以下提示，让它测试刚完成的筛选功能：

```text
使用 Playwright MCP 服务器，在浏览器中打开正在运行的应用 http://localhost:4321，并验证新的游戏筛选功能：

1. 转到游戏页面，并记录列出了多少款游戏。
2. 应用类别筛选器，并确认列表仅显示该类别中的游戏。
3. 清除筛选器，然后应用发行商筛选器，并确认列表仅显示该发行商的游戏。
4. 同时应用类别和发行商筛选器，并确认结果同时满足两项筛选条件。

报告每个步骤的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，按步骤检查并报告结果。请对照 issue 中的验收标准检查总结；如果发现异常，可以提出后续问题，或让 Copilot 在创建拉取请求之前修复代码。

> [!NOTE]
> 测试需要确保应用正在 `http://localhost:4321` 上运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，检查 Copilot 所做的更改。
2. 选择 **+** 图标，将更改暂存。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，例如通过 github.com 或 GitHub 命令行界面（CLI）。不过，既然你已经在使用 GitHub Copilot，不如让它代你创建 PR！它可以找到相关 issue，并创建与该 issue 关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认代理下拉列表中已选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   在存储库中查找与按类别和发行商筛选有关的 issue。为当前分支创建新的拉取请求，并将其关联到正确的 issue。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 进行身份验证；按照提示完成操作即可。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了模型上下文协议（MCP）是什么，以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加了 Playwright MCP 服务器；
- 使用该服务器在发布功能前，通过浏览器手动测试了筛选功能；
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化可访问性审查等专门任务。

## 资源

- [MCP 究竟是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
