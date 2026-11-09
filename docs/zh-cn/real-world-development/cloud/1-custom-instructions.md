---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务有特定做法，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可为 Copilot 提供指导，让它理解不仅要做什么，也要如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令和路径范围指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一项存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不演示修改指令前后的提示效果。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令产生的效果。

## 指令文件

### 场景

Tailspin Toys 团队有一套开发指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用暗色模式并具有现代感。
- 应在代码中通过 TSDoc 文档注释添加说明。
- 每个文件开头都应有一段注释，说明文件用途。

通过指令文件，可以让 Copilot 按照这些实践执行任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解代码风格与要求。可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建这些文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，这些指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊范围；示例项目展示了一种具有代表性的做法。总体而言：

- 在 `copilot-instructions.md` 中集中说明项目级指南，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件和数据层）或特定任务提供专门指令。
- 使用清晰的自然语言，并给出代码应如何编写或不应如何编写的示例。

创建指令文件或使用 AI 没有唯一正确的方法。可以通过实践找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。浏览本项目文件时，你会看到针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。各工作环境的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在需要时说明具体操作。
>
> 如需模板或起点，可查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中针对 Astro、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定指令适用的文件。此处，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 查看此项目针对单元测试的具体指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。查看指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将大型指令集拆分成可复用的小文件，并为 Copilot 提供示例。（这些路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件头注释的规则。

## 添加存储库级标准

本练习中唯一需要编辑的内容，是添加一条项目级规则，要求代码使用 TSDoc 文档注释和文件头注释。云智能体稍后处理此工作环境中的议题时会读取该规则。

编辑前先创建分支。云智能体会从议题目标分支读取指令，但仍应单独提交这项更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，改进 Copilot 的代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前所述，可在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可使用 `*.instructions.md` 为特定语言、文件类型或任务提供指令。适用于整个项目的标准（如文档注释规则）应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习中，Copilot 会从 `main` 分支开始处理分配给它的议题，因此必须先将指令更新合并到 `main`：

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并它。这样，云智能体在下一练习处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你已了解 Copilot 如何从项目指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。接下来几课将审查云智能体创建的拉取请求，以观察这些标准的实际效果。

接下来[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
