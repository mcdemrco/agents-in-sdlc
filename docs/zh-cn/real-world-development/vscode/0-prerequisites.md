---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要完成一些准备工作。你将创建一份 Tailspin Toys 仓库副本，启动一个用于练习的 [codespace][codespaces]，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验仓库

若要创建用于编写代码的仓库副本，需要从[模板][template-repository]创建一个实例。新实例包含实验所需的全部文件，你将在后续练习中使用这些文件。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面上选择**使用此模板**按钮，然后选择**创建新仓库**。

    ![“使用此模板”按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新仓库。

    ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下你创建的仓库路径（**organization-or-user-name/repository-name**），稍后在本实验中会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，系统会自动为你创建一个 GitHub 议题待办事项列表。整个研讨会都会使用这些议题，无需自行创建。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 转到刚创建的仓库。
2. 选择绿色的**代码**按钮。

    ![“代码”按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮以创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。在此期间，你可以先了解 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到此 codespace。现在请将其保留在浏览器标签页中。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都能确保环境具备顺利完成练习所需的全部先决条件。如果你想在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择**在容器中重新打开**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

若要使用 GitHub Copilot Chat 代理模式，IDE 中需要安装 GitHub Copilot Chat 扩展。如果你使用 GitHub Codespace，该扩展应该已经安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat**，然后选择**安装**。

安装扩展后，可能还需要使用 GitHub 帐户进行身份验证才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似 “Hello world” 的消息，然后按 Enter 键。这会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜你，你已创建实验仓库的副本！你还开始创建 codespace，后续编写代码时会用到它。

## 下一步

现在开始使用 Copilot。继续学习[练习 1 - 自定义指令][next-lesson]，了解如何向 Copilot 提供项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
