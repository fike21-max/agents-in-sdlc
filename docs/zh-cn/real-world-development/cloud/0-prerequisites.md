---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云代理练习之前，需要做好准备。你将创建 Tailspin Toys 存储库的个人副本，并启动一个 [Codespace][codespaces]，用于编辑说明文件和审查云代理生成的内容。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板][template-repository]创建一个实例。新实例将包含实验所需的所有文件，你将在整个练习过程中使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上，选择 **使用此模板** 按钮创建自己的存储库副本，然后选择 **创建新存储库**。

    ![使用此模板按钮](../../../_images/ex0-use-template.png)

3. 如果你在 GitHub 或 Microsoft 举办的活动中参加本教程，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下刚创建的存储库路径（**organization-or-user-name/repository-name**），稍后在实验中会用到。

## 创建 Codespace

接下来，你将使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，让你可以直接在浏览器中编写、运行和调试代码。它提供功能完善的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，不过仍比手动安装所有服务快得多！等待期间，可以探索 GitHub Copilot 的其他功能，接下来我们就会介绍它们。

> [!CAUTION]
> 之后的练习中还会用到这个 Codespace。暂时先将它保留在浏览器选项卡中。

> [!NOTE]
> 本教程适用于 Codespace 或本地 [dev container][dev-containers]。两者都会确保环境中已安装顺利完成练习所需的所有先决条件。如果你想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **在容器中重新打开**，VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
## 总结

恭喜，你已创建实验存储库的副本！你还启动了 Codespace 的创建过程，后续会在其中与 Copilot 云代理协作。

## 下一步

接下来，我们将添加云代理要遵循的自定义说明。继续学习[练习 1 - 自定义说明][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
