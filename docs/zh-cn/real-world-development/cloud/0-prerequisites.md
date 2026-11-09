---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要完成环境准备。你将创建 Tailspin Toys 存储库的副本，并启动一个 [Codespace][codespaces]，用于编辑指令文件和审查云智能体的工作。

## 设置实验存储库

基于[模板存储库][template-repository]创建自己的项目副本。新存储库会包含实验所需文件，后续练习都会在其中进行。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可在已授予你 GitHub Copilot 使用权限的组织中创建存储库。

   ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新存储库的路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 Codespace

接下来使用 Codespace 完成实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

   ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建需要几分钟，但比手动安装所有服务快得多。等待期间，可以了解 GitHub Copilot 的其他功能，稍后就会用到。

> [!CAUTION]
> 后续练习还会回到此 Codespace。现在请将它保留在浏览器标签页中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地 [dev container][dev-containers] 中运行。两种环境都已具备顺利完成实验所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

你已创建实验存储库副本，并启动 Codespace 创建流程。后续会在 Codespace 中与 Copilot 云智能体协作。

## 下一步

接下来添加云智能体将遵循的自定义指令。继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 快速入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
