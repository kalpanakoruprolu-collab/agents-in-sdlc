---
title: "第 10 课 - 回顾与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选 issue。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并首个拉取请求 (PR)。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下检查。
- 使用自定义说明引导智能体，定制现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估要求、覆盖率、技能结果和浏览器证据。
- 检查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云端委托的斜杠命令。

## 已交付的内容

工作坊包含两个 PR 里程碑，各自在从更新后的 `main` 创建的独立分支上完成：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选与质量工作流：** 实现筛选、更新说明并将说明应用于功能、定制 `quality-checks` 报告、创建 QA 配置文件并加入相关测试。

从规划筛选功能到打开 PR，你一直使用同一个对话和分支。为简化工作坊，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中检查，以及 Copilot 通过 MCP 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。创建 PR 前，QA 将这些结果与要求和测试覆盖率审查结合起来。

新增测试应解决实际缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前请检查代码和证据；代码更改后，应刷新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可在不同对话中复用这些配置，并根据团队需求调整。说明设定标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。构建前思考方案时使用 **Plan**；需要持续参与小范围更改时使用 **Interactive**；任务范围明确时使用 **Autopilot**。日常编辑可选择更快的模型，复杂工作可选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建*什么*、*为什么*以及*如何*实现，会显著影响结果。

## 继续探索

核心工作流已完成。还可以了解以下 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方法就是继续使用它。可以用于生产代码、个人项目，或那个想了很多年却一直没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续阅读文档。

如果要从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
