---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云代理处理了三项任务，目标是改善用户体验并添加功能。云代理虽然设计为异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有有未关闭拉取请求（PR）的代理任务，了解代理执行的操作，甚至为正在进行的会话提供引导。

在本课中，你将：

- 浏览代理页面，监控云代理任务；
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式也很合理。你将引导代理，让它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云代理任务

现在查看分配给 Copilot 云代理的所有任务的当前状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 留意主窗格和左侧窗格中的任务列表。列表应包含已分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 生成用于修改游戏的 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作。这些操作可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务的各种操作。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 当前的工作状态和执行方式而有所不同。

4. 同时留意右侧显示的拉取请求（PR）窗格。你可以在此查看 PR 和更改的文件，以便进一步监控。

## 引导云代理

现在已了解当前活动任务，接下来要求 Copilot 在处理高对比度模式时，也添加浅色模式切换开关。

1. 选择用于添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到代理完成设置并开始工作。出现类似下方的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中的云代理任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的步骤，再将新请求加入工作流程。

这同样需要几分钟，可以借此暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot 云代理任务的中心。你可以使用这个任务控制中心跟踪所有正在进行和已完成的任务、审查代理执行的工作，甚至重定向正在进行的任务以调整范围或提供额外指导。

你了解了以下内容：

- 使用任务控制中心和代理页面监控云代理任务；
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
