---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 代理模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 会在你的计算机上实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中审查每项差异、接受或拒绝单项更改，并通过后续提示引导对话，以便完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导智能体，添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行时，设计团队又提出了一项需求：他们还希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，可以继续引导现有会话完成这项工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内嵌显示，其中包含差异以及**保留**/**撤消**控件。

1. 从练习 4 返回 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑以及智能体请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内嵌显示：
    - 选择**保留**以接受更改；
    - 选择**撤消**以还原更改，并让智能体再次尝试。
4. 也可以打开聊天顶部的**已更改的文件**指示器，查看智能体在此会话中修改的完整工作集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示**允许**提示。只批准你信任的命令。

5. 等待智能体完成高对比度模式的实现。完成后，你会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，可以在同一对话中继续添加浅色模式切换开关。由于仍在同一个聊天会话中，智能体会保留先前工作的上下文，包括修改的组件、使用的样式方案以及设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会接着此前的工作，探索现有更改并提出浅色模式的新增内容。
3. 像审查高对比度模式一样，逐项审查新编辑。使用**保留**或**撤消**处理每个差异。
4. 如果智能体的方向不符合预期，例如试图以不喜欢的方式重构切换 UI，可以选择聊天输入框旁的**停止**按钮将其停止，然后发送说明要求的后续消息。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板中”）或扩展范围（“为两个切换开关都添加键盘快捷方式”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开**源代码管理**视图。
2. 审查更改文件的完整列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认其中采用了自定义智能体推荐的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在源代码管理面板中暂存并提交更改。下一课会发布这些更改。

## 总结和下一步

你使用 Copilot Chat 代理模式监控并引导了正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留会话上下文。

本课介绍了以下内容：

- 实时监控智能体的工作；
- 接受或拒绝单项编辑；
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍工作作为拉取请求发布，并审查本实验中本地自定义智能体完成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
