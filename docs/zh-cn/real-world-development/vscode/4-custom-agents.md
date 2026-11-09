---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以通过仓库的 `.github/agents` 文件夹中的 Markdown 文件定义智能体，为 Copilot 提供聚焦的指令、最佳实践、编码模式和特定领域知识，从而更有效地完成特定类型的工作。团队可以将专业知识编写成可重复使用的智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件的 YAML frontmatter 至少包含 `name` 和 `description` 字段，后接用于定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在逻辑上有一些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的成员，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站，并可在工作过程中调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，需要在目标环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，由于文本与背景颜色之间的对比度不足，有些用户觉得当前深色主题难以阅读。为解决此无障碍问题，设计团队要求实现一种可供用户开启或关闭的高对比度模式。

由于无障碍至关重要，你希望尽快完成实现。接下来将使用自定义智能体生成此功能。

在本练习中，你将：

- 查看现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

已为你创建了一个无障碍自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 创建新分支，用于无障碍相关工作（Exercise 3 中的筛选 PR 会保留在单独的分支上）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接下来，查看其余部分，了解其中介绍的内容：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，Copilot Chat 会根据该智能体的指令和工具限定会话范围。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板，然后选择**新聊天**以开始一段干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（其中显示当前智能体；默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并应用到项目文件。每项编辑都会在聊天中内嵌显示，其中包含文件路径和可供审查的差异。
6. 随着编辑逐步应用，**已更改的文件**指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改仓库内容，包括 Astro 组件、CSS 和相关测试。

你将在下一课中审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。你可以用自定义智能体将团队的专业知识和标准编写成可重复使用的指令，从而引导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来，你将[监控并引导智能体的工作][next-lesson]，在工作进行时审查更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
