---
title: "第 9 课 - GitHub Copilot CLI 实用工具"
description: "探索用于管理上下文、选择模型、共享会话和可选地委托任务到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令，可用于高级功能、内部信息和其他配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在再来了解一些实用命令。

本课将完成以下目标：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、worktree 和委托任务到 cloud agent 的可选命令。

## 场景

CLI 核心工作流已经完成。现在了解其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令检查或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统说明、工具、消息和剩余空间各自占用的上下文。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。使用 `/compact` 可自行选择时间。切换到无关任务并希望开启全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出模型并供你选择。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于提升每个人使用 AI 工具的能力。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先进行检查。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展或委托工作

工作坊核心内容已经完成。Copilot CLI 还为更大型的任务提供以下命令：

- `/fleet` 可将独立子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git worktree。
- `/delegate` 可将任务发送给 Copilot cloud agent；它会异步工作，并可能打开拉取请求。

这些命令可能会创建其他 worktree 或远程工作，因此属于可选内容。尝试前先开始新的、范围明确的任务，并按常规流程检查结果。要深入了解智能体异步工作，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并查看幕后情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、worktree 和委托到 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令可供探索。现在，[回顾所学内容][next-lesson]并了解后续步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
