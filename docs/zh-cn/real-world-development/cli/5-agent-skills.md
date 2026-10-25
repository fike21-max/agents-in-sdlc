---
title: "第 5 课 - 自定义并使用质量检查技能"
description: "了解现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是把代码写出来。我们已经手动验证代码能否运行，并使用指令文件确保代码符合标准。但测试、lint 以及持续集成 (CI) 的其他环节该如何处理？

这类任务最适合交给**智能体技能**！技能可以帮助 Copilot 正确执行这些操作。

本课将介绍如何：

- 探索现有的 `quality-checks` 技能。
- 自定义结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和支持资源打包在一起，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件包含定义技能名称和描述的 frontmatter、技能概述以及调用时机的说明。文件夹还可以包含脚本和其他供技能使用的资源子文件夹。

> [!NOTE]
> 技能不一定需要其他文件夹和文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

先看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述会帮助 Copilot 判断何时调用该技能。
3. 阅读说明，了解它们如何引导 Copilot 完成测试和 lint 流程。
4. 注意该技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

技能可以通过 Copilot CLI 直接调用，也可以用自然语言调用。现在让 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式中的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告更清晰地说明运行了什么、是否成功，以及工具实际报告了什么。现在更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，运行技能查看效果。Copilot CLI 可以重新加载已编辑的技能，无需重启对话。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次的报告进行比较。

## 总结与后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

此更改将与筛选功能一起纳入功能 PR。接下来，你将让 Copilot 直接操作网站，[通过 Playwright MCP 服务器][next-lesson]探索网站。

## 更多技能示例

以下社区示例仅供参考，不是本课的额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
