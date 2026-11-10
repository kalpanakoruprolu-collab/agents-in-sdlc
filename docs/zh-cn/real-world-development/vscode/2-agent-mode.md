---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……这样的工作还会不断重复。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在集成开发环境（IDE）中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云代理的名称相似，但它们针对不同的使用场景。代理模式在 IDE 中执行任务，便于快速反馈和交互。云代理则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 代理模式探索项目、识别相关文件并协调完成多项更改；
- 使用 GitHub Copilot 代理模式跨用户界面（UI）和数据层实现新功能；
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和发行商筛选游戏。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站之前，先了解其当前功能。

该网站是一个以开发者为主题的桌游众筹平台，用户可以浏览游戏列表和详情。网站是一个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能由使用标准文档对象模型（DOM）API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应已切回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从终端选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，此时 Web 服务器正在 codespace 中运行！

### 浏览网站

网站运行后，可以查看其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 查看待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始新的对话。不同练习使用的模式和模型可能不同，相关步骤会在适用时说明。

网站的初始实现已可正常使用，但还需要添加更多功能。先查看待办列表。使用模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨多个文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。构建本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，方便完成练习。不过，由于大语言模型（LLM）的输出具有概率性，你看到的建议可能与工作坊中的示例不同。这是正常现象。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办列表中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还会根据它认为对用户最有用的程度，为这些事项确定优先级。

## 查看指令文件

让代理生成代码前，先查看将用于向 Copilot 提供上下文的指令文件。本练习将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向不同文件类型专用指令的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样便于重复使用内容、将复杂指令拆分成更易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项更改：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 代理模式可以帮你完成这些工作。现在开始添加此功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，添加指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 作为模型。
5. 使用以下提示词，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你应能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行期间选择它们。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中提示确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会反复生成代码并运行测试，直到完成任务且未检测到错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 使用 Copilot 代理模式探索项目、识别相关文件并协调完成多项更改；
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断增长，需要添加分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示词时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 考虑分页方式，例如是否允许用户选择每页大小，或将其设为固定值；
- 编写提示词时，向 Copilot 提供实现所需的指导；
- 可能需要通过提供更改请求和上下文来迭代改进 Copilot 的结果。这是与 Copilot 协作时的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
