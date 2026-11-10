---
title: "第 8 课 - 创建并合并功能拉取请求 (PR)"
description: "一起检查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在将它们放在一起检查并打开拉取请求 (PR)。星级评分 PR 是你手动合并的；这次将由 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立 PR 中。为简化工作坊，筛选和质量流程的全部工作都在同一个对话和分支中完成，并放入此 PR。

本课将完成以下目标：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理 PR 的剩余工作，同时由开发者控制是否授权合并。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 的阻塞事项，包括修复失败的持续集成 (CI) 检查、回复审查意见以及在需要时执行变基；如果存储库允许，随后会启用 GitHub 自动合并。

此前由你选择 **Merge pull request**。Agent Merge 可以承担这项工作。启用前，请先检查内容并确认可以合并。

## 使用 Agent Merge 管理 PR

代码全部完成后，一起审查更改、创建 PR，并让 Agent Merge 处理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 在选项列表中选择 **Default**，然后选择 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 激活 agent merge。
6. Copilot 将监视 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认看到 Copilot 发出的类似 "PR #14 was squash-merged successfully." 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或缺失权限。如果遇到阻塞，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程的多个部分，包括代码生成、代码测试和验证，以及现在的拉取请求流程。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索其他实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享以及可选的云端委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
