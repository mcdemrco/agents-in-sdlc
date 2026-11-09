---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义指令][previous-lesson]

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求 (PR)，供开发者审查。这样可以委派繁琐任务、加快开发进度，让开发者专注于更重要的工作。

本练习将介绍 Copilot 云智能体的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初受聘开发网站的承包团队没有留下足够的文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。目前不要求实现经过完善的功能，只需提供可用于用户体验验收测试的版本。此需求目前阻塞了工作，但优先级低于其他议题。

这类任务很容易被延后，适合分配给 Copilot 云智能体异步处理。开发者可以先做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式开始任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 PR 记录进度、设置工作环境并开始处理任务。你可以在任务进行中或完成后查看 Copilot 会话。解决方案可供审查时，Copilot 会在 PR 中标记你。

## 明确界定任务范围的重要性

GitHub Copilot 并非万能工具，使用它并不能省去思考。即使看似简单的操作，也可能包含复杂细节，因此要谨慎界定云智能体任务范围，并遵循[云智能体最佳实践][cloud-agent-best-practices]。像与 AI 结对编程一样，分阶段工作、学习、试验并调整。加入生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中的指令文件，以便稍后观察它们的影响。

在 GitHub 网页界面或 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体。审查生成的 PR 时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释；这些内容来自上述指令文件。后续练习会再次检查它们。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本。将任务分配给以类似软件工程师方式工作的 Copilot 时也一样。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建专用设置工作流 `.github/workflows/copilot-setup-steps.yml`，在它开始工作前运行，以便安装所需开发工具和依赖项。本实验已提前配置该工作流，确保 Copilot 可以使用 Node.js、项目依赖、端到端测试所需的浏览器，以及已迁移和填充数据的 SQLite 数据库。该项目是单个 Astro 应用：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行*你的步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用程序设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务常被搁置，会拖慢开发并增加维护和引入新成员的难度。Copilot 擅长生成文档，因此适合将这项任务分配给云智能体；稍后可回来审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看 Copilot 即将执行的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并制定方案，然后生成代码。

这可能需要几分钟。云智能体的主要优势之一是异步执行任务，让你可以把精力放在其他工作上。接下来利用这一点，再分配一项任务，然后转而编写应用功能。

## 在游戏详情页添加相关游戏

云智能体可以并行处理不同工作，让你专注于其他任务。即使设计团队要求的相关游戏功能不需要很长时间，委派给云智能体仍可腾出时间处理其他工作。现在分配这项任务：

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在后台处理第二项请求。和软件工程师类似，云智能体会独立工作；你无需持续监控，完成后再审查即可。接下来转到自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体异步处理，用它解决技术债务、开发功能或协助迁移代码框架。

本课介绍了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定任务范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，可以开始创建和使用自定义智能体。[云智能体也支持模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
