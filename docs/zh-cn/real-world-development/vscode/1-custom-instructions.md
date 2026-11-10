---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档规范如何通过存储库自定义指令和按路径限定范围的指令文件传达给 Copilot；
- 在当前指令的基础上，生成筛选功能的第一个数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范；
- 再次发送相同提示词，观察生成的代码如何采用新规范；
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指引。

> [!CAUTION]
> 生成的代码可能不符合你设置的部分标准。Copilot 的输出具有非确定性。本练习的重点是观察更新指令前后行为的变化趋势，而不是要求输出逐字一致。

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

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或命令行界面（CLI）请求相关的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
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
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目中的指令文件时，你可能会发现有些文件适用于多种任务，包括[用户界面更新][ui-instructions]和 [Astro][astro-instructions]。
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

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始新的对话。不同练习使用的模式和模型可能不同，相关步骤会在适用时说明。

要查看自定义指令的影响，先在当前指令文件不变的情况下发送提示词。稍后更新指令后，再次发送相同提示词并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 可以应用文件更改。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，有时即使没有指令，它也会添加文档注释。若出现这种情况也没关系，本练习重点仍是观察添加指令后行为的一致性改进。

## 添加新的存储库规范

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

## 重新发送提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供用于从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目规范，并将它应用于实际代码。下一练习会在此基础上继续构建。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，采用相同的源代码管理流程。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

推送分支后，需要创建拉取请求（PR）并关联待办列表中的文档规范议题。你可以手动完成，也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理。通过提示词让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按照提示完成授权。后续练习中会介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中了 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找议题并创建 PR。
5. 如果 Copilot 提示你允许它代表你执行 GitHub 操作，请检查命令并酌情选择 **Allow**。
6. 创建 PR 后，提示 Copilot 将 PR 合并到 `main` 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示你允许它执行 GitHub 操作和 shell 命令，请检查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示词，观察 Copilot 根据**现有**指令生成的结果；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范；
- 再次发送相同提示词，观察建议的代码如何采用新规范；
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
