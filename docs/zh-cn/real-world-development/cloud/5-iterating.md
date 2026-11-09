---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你已与 GitHub Copilot 协作完成多项改进用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换。现在检查代码更改；如有需要，向 Copilot 提供反馈以改进结果。

### 场景

正如前面多次提到的，使用生成式人工智能 (AI) 不会改变软件设计和开发运维 (DevOps) 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来审查 Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再允许团队其他成员审查。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此采用了安全限制：

- Copilot 只能读取存储库，并且只能写入它用于工作的分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 所有 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，检查 GitHub Copilot 云智能体创建的第一个 PR，即为代码添加文档的 PR。

> [!NOTE]
> 查看拉取请求 (PR) 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似于 **Add missing documentation** 的 PR。标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。此时请等待 Copilot 完成；可能需要几分钟，可以先休息或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。检查 TSDoc 文档注释、TypeScript 约定和文件头注释；这些内容来自练习 2 中查看的自定义指令文件。
6. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 检查队列中。正常情况下，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在 PR 中联系团队成员一样，在评论或代码行内评论中标记 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法给出固定提示示例。你可以要求 Copilot：

- 在每个代码文件开头添加简短的文件用途注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 添加评论并像标记其他用户一样标记 **@copilot**，请求修改生成的文档。可以使用上述建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并应用更改。选择 **Back to pull request** 可返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

3. Copilot 完成更改后，PR 中应出现新的提交。
4. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将草稿 PR 标记为准备审查，再合并到 `main`。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在检查 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR：

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。标题也可能更具体。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应出现在 PR 检查队列中。正常情况下，单个 Astro 应用的项目检查将通过；此过程可能需要几分钟。
9. **可选：**在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端并运行以下命令；将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 `copilot/fix-8`：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以通过评论继续迭代。例如，可以要求调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义指令）。在 **Conversation** 选项卡中添加评论，Copilot 就会收到请求并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中在控制中心追加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成；可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 检查实现，尤其关注：
   - 用于切换模式的用户界面 (UI) 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改。
   - 无障碍属性（无障碍富互联网应用 (ARIA) 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 检查队列中。正常情况下，单个 Astro 应用的项目检查将通过；此过程可能需要几分钟。
10. **可选：**在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置会在页面重新加载后保留。

无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理前两个 PR 一样，在评论中标记 **@copilot** 并要求它更新。

## 可选练习：继续委派任务

云智能体适合处理真实待办事项，让你可以转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表添加分页。
- 为数据访问助手添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的练习。在这些课程中，你：

- **查看了存储库附带的自定义指令文件**，以了解它们如何影响云智能体的输出。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **将 Copilot 智能体页面用作控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体创建的 PR**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体工作环境的练习已完成。其他工作环境可以帮助你继续探索：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在集成开发环境 (IDE) 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义质量保证 (QA) 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
