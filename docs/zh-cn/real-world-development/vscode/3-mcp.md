---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用代理模式构建了筛选功能。在创建拉取请求之前，应先确认该功能在浏览器中能正常运行。此处无需手动浏览应用进行检查，而是连接 **Playwright MCP 服务器**，让 Copilot 控制真实浏览器并测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用此服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式能够访问编辑器之外的资源时，它的能力会大幅增强。模型上下文协议（MCP）让 Copilot 能够做到这一点，它是智能体与外部工具和服务通信的标准方式。

![代理模式与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而访问最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

以下是几个常用的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 仓库的 API。AI 智能体可以使用它执行创建仓库、更新现有仓库以及管理议题和拉取请求等操作。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以使用它导航网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器，可用于访问不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，以便于查找和参与 MCP 生态系统。

> [!CAUTION]
> 在安全方面，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用 MCP 服务器前，请仔细检查其源代码、验证发布者，并考虑安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目由项目模板提供，因此 Copilot 在上一课中可以读取你的议题待办事项列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其中一个工具时让你使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可用于测试此功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 条目并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必要的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 随后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示**启动**操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。无需手动浏览应用检查工作，智能体可以打开页面、导航、应用筛选条件、读取结果并向你总结观察到的内容。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会根据带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同，因此快速的功能检查也能顺便进行简单的无障碍检查。

服务器已连接且应用正在运行时，向 Copilot 发送以下提示，让它测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成检查，然后报告结果。根据议题中的验收条件查看它的总结。如果发现问题，可以继续询问，或让它修复代码后再创建拉取请求。

> [!NOTE]
> 测试需要应用在 `http://localhost:4321` 上运行。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队进行审查。首先发布 `filtering-vscode` 分支。

1. 转到 codespace 中的**源代码管理**面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标以暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的源代码管理面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择**发布**，将分支推送到仓库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面（CLI）。不过，既然已在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关议题，并创建与议题关联的 PR。

1. 转到 Copilot Chat 面板并选择**新聊天**，开始新的会话。
2. 确认在智能体下拉列表中选中 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择**继续**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你使用 GitHub 进行身份验证；按照提示完成操作。
5. 留意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接以查看拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！在本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用该服务器在发布前通过浏览器手动测试筛选功能；
- 发布了分支，并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，创建一个[自定义智能体][next-lesson]，以便简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
