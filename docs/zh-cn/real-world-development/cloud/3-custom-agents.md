---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，为其提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验整理为可重复使用的代理，例如确保符合 [Web 内容无障碍指南（WCAG）][wcag]要求的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 路径中定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段；后面是定义代理行为、专业知识和指令的 Markdown 提示词。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]之间有一定的逻辑重叠。两者主要通过 Markdown 文件定义，并向 AI 说明如何执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 创建自定义代理没有唯一的正确方法。与 AI 的其他用法一样，需要针对自己的环境和场景不断测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将探索如何使用自定义代理：

- 了解自定义代理的定义方式；
- 将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此要尽快实现此功能。你将使用自定义代理生成相应功能。

## 查看无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 创建议题并分配代理

任务控制中心是管理环境中所有代理的中心位置。你可以将任务分配给 Copilot 云代理、监控任务并提供额外指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开议题创建对话框。
4. 选择 **Blank issue** 创建议题。
5. 在 **Title** 中输入 `Add high contrast mode to website`。
6. 在 **Description** 中输入：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配截图，其中突出显示了自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，它们是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队专业知识和标准整理成可重复使用的代理，更有效地引导 Copilot 完成特定类型的工作。

你了解了以下内容：

- 自定义代理的定义方式；
- 将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，从任务控制中心[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为在组织中使用自定义代理做准备][org-custom-agents]
- [为在企业中使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
