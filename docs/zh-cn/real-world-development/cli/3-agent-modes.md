---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能，但更大的改动需要更完善的流程。GitHub Copilot CLI 能融入组织现有的工作流，帮助团队以恰当的方式构建正确的功能。本系列课程将介绍常见的智能体驱动开发流程：从 issue 创建功能、验证代码、确认功能行为，最终成功合并。

> [!NOTE]
> 后续功能工作都在同一对话和分支中继续。通常，不同文件类型的工作会使用不同分支或 PR；这里为聚焦核心概念而简化流程。

本课将完成以下目标：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续开发该功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后允许 Autopilot 在限定范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则，反而使这些原则更加重要。许多开发者遵循类似流程：

1. 从描述工作内容的 issue 开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查和持续集成后，再合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述做法类似。

遵循标准流程可以确保 AI 生成的代码满足要求，并经过与手写代码相同的审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并不断迭代，直至任务完成。

先在 Plan 模式下查看计划，然后使用 Autopilot 实现。

## 从 issue 开始

开始筛选工作前，返回 Codespace，确保存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示，然后返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub issue 或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以让你在交给 Copilot 前确定功能实现方案或任务步骤。复杂任务尤其值得先花时间规划。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方案。

3. Copilot 可能会询问筛选行为。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，使用 Autopilot 实施。Autopilot 会让 Copilot 反复处理问题，直至它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为完成。

## 检查并验证实现

与其他代码一样，生成的代码也需要在合并前审查。还要运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与 issue 和规划时的决定进行比较。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出 diff 视图。
5. 查看项目检查结果，并要求 Copilot 解决失败项。

## 试用新功能

代码看起来不错，但能否运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试类别筛选、发行商筛选，以及计划中约定的组合方式。
4. 确认重置和无结果时的行为符合 issue 和双方的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并检查了功能。本课中，你：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下定义了要求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义说明][next-lesson]进一步了解代码生成，确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
