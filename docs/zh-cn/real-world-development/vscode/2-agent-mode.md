---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再次修改……工作内容往往不止于此。

Copilot 代理模式正适合处理这类任务。

Copilot 代理模式可在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。下面了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但它们针对两种不同的体验。代理模式在 IDE 中执行任务，可快速反馈并与用户交互。云智能体则像团队中的同伴程序员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 代理模式在 UI 和数据层中实现新功能；
- 在将更改合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望让用户按类别和发行商筛选游戏。上一课已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先查看 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的桌游众筹平台，可列出游戏并显示其详细信息。它是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中，交互功能则通过范围较小的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一课结束后，你应该已经切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，你会看到类似以下内容的横幅，其中显示服务器 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从 Codespace 终端选择 localhost URL 的链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，网站现在由此运行！

### 浏览网站

网站运行后，可以查看其功能。主要功能包括：

- **主页**：显示桌游列表，包括游戏标题、图片和说明。
- **游戏详细信息页**：选择游戏后，会打开详细信息页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择**新聊天**以开始一段干净的对话。模式和模型选择因练习而异；每个步骤会在相关处说明。

网站的初始实现已可正常运行，但我们想增加新功能。先查看待办事项列表。通过模板设置项目时，系统已自动创建议题待办事项列表。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。选择 Agent 后，Copilot 会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此课程中包含了该模型，以便让代码建议尽可能一致。不过，由于 LLM 具有概率性，你收到的建议可能与课程示例不同。这完全正常，也是预期的情况。

> [!NOTE]
> 由于模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办事项列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择**继续**以运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还会根据它认为对用户最有用的程度，为这些项目排定优先级。

## 查看指令文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的指令文件。这里会使用 [UI（用户界面）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含在网站中添加功能时应遵循的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍方面的原则；
   - 指向多种文件类型专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件既可以引用其他指令文件，也可以引用项目中的文件。路径相对于指令文件所在位置。这样可以重复使用指导，将较大的指令拆分成更小、更易管理的部分，并提供示例和模板供 Copilot 参考。

## 实现筛选功能

完成筛选至少需要对应用进行三项单独的更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面，以加入筛选 UI。

此外，测试需要运行并通过后，才能将更改合并到代码库。Copilot 代理模式可以完成这些任务，下面开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择**新聊天**。
2. 选择**添加上下文**、**指令**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能相关的文件。你应该会看到它查找数据层辅助函数、UI 和测试，然后开始修改这些文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现**保留**和**撤消**按钮。Copilot 完成后，所有更改都会显示**保留**或**撤消**选项，因此无需在工作仍在进行时选择这些按钮。

7. 按 Copilot 的提示选择**继续**以运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至会看到测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![完整的 Copilot 代理模式聊天会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或任何 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，查看新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择**保留**。

## 总结和下一步

恭喜！在本练习中，你了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你还了解了如何：

- 使用 GitHub Copilot 代理模式在 UI 和数据层中实现新功能；
- 使用 Copilot 代理模式探索项目、识别相关文件并协调更改；
- 在将更改合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并为此功能创建拉取请求。

### 扩展探索练习 - 实现分页

随着游戏列表不断增长，网站需要支持分页。运用本练习中学到的技能，提示 Copilot 为网站实现分页。编写提示和代码时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件；
- 考虑分页方式：允许用户选择每页大小，还是将其固定；
- 编写提示时，提供 Copilot 所需的指导，确保实现符合你的要求；
- 可能需要与 GitHub Copilot 多次交互，要求它进行更改并提供上下文。这是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
