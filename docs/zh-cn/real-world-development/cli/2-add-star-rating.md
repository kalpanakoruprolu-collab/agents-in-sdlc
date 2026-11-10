---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并将其作为首个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以首次修改项目了。这次改动范围很小：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让智能体显示已有评分、检查更改，并将其作为首个拉取请求合并。

本课将完成以下目标：

- 在功能分支上启动专注于任务的 Copilot 对话。
- 请求智能体对项目进行小幅修改。
- 使用 `/diff` 检查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并首个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，详情页已经显示该评分。不过，主页上的游戏卡片仅显示标题、类别、发行商和描述。作为热身，你将让智能体在每张卡片上显示已有评分。这项小而独立的改动很适合首次使用。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作空间。与 Copilot 应用不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用 worktree。保存的对话可供日后继续讨论；文件和分支仍是磁盘上的常规 Git 状态。

对话中包含你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

启动新对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd> 启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。你已使用 Copilot CLI 添加了新功能！

## 检查 diff

合并前应检查所有 AI 生成的更改，即使改动很小也不例外。现在在 Copilot CLI 中检查更改。

1. 输入 `/diff`，检查每个已更改的文件。
2. 确认有评分时游戏卡片显示数值；`starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 检查 Copilot 运行的检查结果，并要求它修复失败项。
5. 检查完成后，按 <kbd>Esc</kbd> 退出 diff 视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的结果具有概率性，并非确定不变。因此，实际代码可能不同。请检查行为，而不要期待某一种具体实现。

## 检查更改

不能只阅读代码就假定它能正常运行。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五分制评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并首个拉取请求

功能已创建。现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 请求默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。创建 PR 时，Copilot CLI 会推送现有提交并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查已更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了首项更改。本课中，你：

- 在功能分支上启动了专注于任务的 Copilot 对话。
- 引导智能体修改了游戏卡片。
- 使用 `/diff` 检查了更改。
- 运行应用，并在转发的浏览器中确认了星级评分。
- 创建并合并了首个拉取请求。

接下来，你将[从筛选 issue 入手，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
