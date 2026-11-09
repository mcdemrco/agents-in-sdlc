---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体会异步自主工作，但仍需要监控任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，也可以引导仍在进行的会话。

在本课中，你将：

- 通过智能体页面监控云智能体任务。
- 引导进行中的会话，要求添加其他功能。

## 场景

团队要求添加高对比度模式后，意识到也应提供浅色模式。网站正在更新样式并添加切换功能，因此适合在同一任务中加入浅色模式。你将引导智能体同时实现浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务当前状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成所分配任务的各项步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 的工作状态和采用的方案而异。

4. 查看右侧显示的 PR 区域，了解 PR 和已更改文件，以便进一步监控。

## 引导云智能体

查看活动任务后，在 Copilot 处理高对比度模式时，要求它同时添加浅色模式：

1. 选择添加高对比度模式的会话。会话标题取决于 Copilot 的命名和当前工作状态。

   ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。此时会出现类似下文所示的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot 智能体页面中突出显示“Steer active session while copilot is working”对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已收到提示，并将其纳入任务流程。

## 等待 Copilot 完成工作

Copilot 会继续处理更新后的任务，并在完成当前步骤后将新要求纳入工作流程。

这可能需要几分钟。等待时，可以回顾此前学到的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这个中心页面可用于监控和引导 GitHub Copilot 云智能体任务。你可以跟踪活动和已完成的任务、审查工作，也可以调整进行中任务的范围或提供额外指导。

本课中，你：

- 使用控制中心和智能体页面监控云智能体任务。
- 引导进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
