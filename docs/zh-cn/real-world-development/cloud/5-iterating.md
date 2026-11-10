---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在整个实验过程中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建可供迭代改进的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在来查看代码更改，并在必要时向 Copilot 提供反馈，进一步改进其工作。

### 场景

如前所述，生成式人工智能（AI）并不会改变软件设计和开发运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循常规的 DevOps 流程。接下来，在为团队其他成员启用审查之前，先查看 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云代理

Copilot 云代理会异步且无人监督地执行任务，因此设有一些安全限制，确保工作安全：

- Copilot 对存储库只有读取权限，对用于提交代码的分支才有写入权限。
- 云代理在 GitHub Actions 中运行，并创建独立的临时环境来工作。
- GitHub Actions 工作流必须经人工批准才能运行。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审查生成的文档

先查看 GitHub Copilot 云代理创建的第一个拉取请求（PR），它为代码添加了文档。你将在 GitHub.com 上使用标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源的访问受限，包括调用外部 MCP 服务器。你也可以[自定义或禁用 Copilot 云代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，也可能是其他更完善的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以稍作休息或回顾迄今为止学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理拉取请求并非单向过程。你可以像提及团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，无法规定应提出的具体要求。可以让 Copilot 更新的内容包括：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论以请求更改生成的文档，并像提及其他用户一样提及 **@copilot**。可以采用上面的建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡查看更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转换为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，也可能是其他更完善的标题。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分排队。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能！与之前一样，可以通过迭代让 Copilot 更新结果。例如，可以要求 Copilot 调整显示的相关游戏数量，或确保添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义代理实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，也可能是其他更完善的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，重点留意：
   - 用于切换模式的用户界面（UI）组件；
   - 使用 local storage 保存用户偏好；
   - 高对比度和浅色模式的层叠样式表（CSS）或其他样式更改；
   - 无障碍属性（可访问的富互联网应用（ARIA）标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能符合预期，并且重新加载页面后设置仍然有效。

留意无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云代理适合处理实际待办事项，让你能把注意力转向其他工作。可以创建几个议题并分配给 Copilot，养成这种工作习惯。以下是一些建议：

- 在游戏详情页创建支持者意向表单；
- 为游戏列表页添加分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理工作环境中的练习。在这些课程中，你：

- **查看了此存储库提供的自定义指令文件**，以便之后观察它们对云代理输出的影响。
- **将议题分配给 Copilot 云代理**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用了无障碍自定义代理**，添加高对比度和浅色模式切换开关。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代改进了云代理的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和下一步

你已完成云代理工作环境中的练习。想继续探索，可以尝试其他工作环境，补充本次实践的内容：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 直接在集成开发环境（IDE）中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[命令行界面（CLI）工作环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、自定义质量保证（QA）代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接，完成两项经过审查的更改。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云代理；
- 为其他领域创建自定义代理；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
