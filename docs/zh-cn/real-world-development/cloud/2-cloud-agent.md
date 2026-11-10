---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给代理，加快开发进程，让开发者专注于更重要的工作。

你将探索如何使用 Copilot 云代理：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上，代码中完全缺少文档。首先，他们希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，也就是属于同一类别的其他游戏。目前不需要完善的实现，只要可以用于用户体验验收测试即可。这项工作目前受阻，但优先级低于其他议题。

这两项工作都可能很快被降级，可以交给 Copilot 云代理处理。代理可在后台异步工作，让开发者专注于其他任务，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。与人类开发者协作类似，可以通过多种方式分配工作，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求（PR）以跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 指令范围明确的重要性

GitHub Copilot 并非万能，不能替你思考。即使看似简单的操作，深入分析后也会发现其复杂性，因此要[谨慎确定 Copilot 云代理任务的范围][cloud-agent-best-practices]。把它当作人工智能（AI）结对编程助手：分阶段工作、学习、尝试并不断调整。加入生成式人工智能（AI）并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的实际效果；否则，现在可以快速查看一下。将工作分配给 Copilot 云代理前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云代理时，留意它创建的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自上述指令文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也是如此，因为它的工作方式类似于软件工程师（SWE）。

云代理使用 [GitHub Actions][github-actions] 执行任务。你可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，并在云代理开始工作前运行，以便它访问所需的开发工具和依赖项。实验已预先配置好该工作流，方便你顺利完成实验。它会为这个 Astro 单体应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及完成迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 例如，如果设置步骤需要克隆存储库以安装依赖项，则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目的文档要么过时，要么缺失。这类技术债务常被忽视，降低工作效率，也增加维护代码库或帮助新开发者加入团队的难度。Copilot 擅长创建文档，因此这正是交给 Copilot 云代理处理的合适任务。它会在后台生成所需文档，之后的练习中你会审查其成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开议题创建对话框。
4. 选择 **Blank issue** 创建议题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态指示器更新。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码和理解当前状态、暂停思考并决定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让我们能处理其他工作。接下来就利用这一点：再分配一项任务给 Copilot 云代理，同时开始为应用编写新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云代理的一大优势是可以拆分工作：你专注于一组任务，代理处理另一组。为设计团队添加相关游戏部分可能不需要很长时间，但也可以把这段时间用于其他任务。现在就把工作分配给 Copilot 云代理！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开议题创建对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入：`Show related games on the game details page`
6. 在 **Description** 中输入：

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

很快，你会看到议题的第一条评论旁边出现一组 👀，表示 Copilot 已开始处理！

![Copilot 用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二个请求！云代理的工作方式类似于软件工程师（SWE），因此不必主动监控，而是在工作完成后进行审查。现在来了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。可以将议题分配给云代理，让它异步处理工作。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你了解了以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

Copilot 云代理正在后台处理任务，现在可以开始创建和使用自定义代理。[Copilot 云代理还支持模型上下文协议（MCP）服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
