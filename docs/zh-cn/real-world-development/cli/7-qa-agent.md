---
title: "第 7 课 - 创建并使用质量保证 (QA) 智能体"
description: "创建 QA 自定义智能体，将 issue 要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到具有明确定义质量保证 (QA) 流程的自定义智能体中。

本课将完成以下目标：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选 issue 检查其发现。

## 场景

Tailspin Toys 希望在打开拉取请求 (PR) 前，一致地检查要求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义配置各有用途：存储库说明描述团队标准；`quality-checks` 技能整理可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估要求并报告发现。它不会取代其他配置，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该配置将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免不完整或范围过大的智能体修改你只想验证的功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但添加后可提供清晰的显示名称。
3. 阅读配置文件说明，确认 QA 从要求开始、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或打开 PR。
5. 如果生成的配置文件缺少上述职责或边界，请先要求默认智能体修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目智能体。恢复同一个筛选对话并选择 QA，使其能够使用已有上下文中的 issue 和规划决策。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但上述命令仍会将其激活。

2. 使用以下提示让 QA 智能体检查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已添加可复用的专用角色，并检查了它的工作。本课中，你：

- 了解了自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选 issue 检查其发现。

实现、技能更新、QA 配置文件、测试和验证报告均已准备好等待审查。接下来，[将它们汇总到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
