---
title: "练习 1 - 自定义指令（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档规范如何通过存储库自定义指令和按路径限定范围的指令文件传达给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作环境不同，本练习不会使用“更新前/更新后”的提示词。Copilot 云代理会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云代理创建的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 和许多优秀的软件开发团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。借助它可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至喜欢的注释形式。既可以为整个存储库创建指令，也可以为特定文件类型或任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或 CLI 请求相关的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。例如，可为 TypeScript 或 Astro 等语言编写指南，也可为创建 UI 组件或单元测试等任务编写指南。

> [!NOTE]
> 在集成开发环境（IDE）中工作时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指引限定在项目级内容，例如项目描述、项目结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指引。
- 使用自然语言，确保指引清晰，并提供代码示例说明应如何编写（以及不应如何编写）。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过尝试和调整，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目中的指令文件时，你可能会发现有些文件适用于多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前工作环境的相关课程会在适用时介绍具体操作。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库提供的指令文件：其中有一个核心 `copilot-instructions.md`，还有一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下还有 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。留意针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 留意本项目专门用于创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码规范，但尚未要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库规范

接下来要编辑一个内容：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云代理稍后处理本工作环境中分配的议题时会读取这些规则。

编辑前，先创建一个分支（云代理会读取议题目标分支中的指令；这里仍按规范提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库添加编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应在第 27 行附近。留意其中记录了项目的编码规范，但还没有关于代码文档的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务添加指令。项目级标准（例如刚添加的文档注释规则）适合放在存储库级指令文件中。

## 提交、推送并合并指令更新

云代理会读取议题目标分支中的指令。下一练习中，你会将议题分配给 Copilot，而它会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云代理才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并将其合并。下一练习中云代理处理分配给它的议题时，就会读取这些指令。

> [!TIP]
> 如果想在本工作坊中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 `.github/copilot-instructions.md` 中添加了新的存储库级规范。在本工作环境的后续练习中，审查云代理创建的拉取请求时，可以看到这条规范的实际作用。

接下来，[将第一个议题分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
