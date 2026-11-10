---
title: "练习 4 - 自定义代理"
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

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此要尽快实现此功能。你将使用自定义代理生成相应功能。

在本练习中，你将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

为无障碍功能创建独立于筛选功能拉取请求（PR）的分支。

1. 返回 codespace，打开终端，并从 `main` 分支创建新分支：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

2. 打开 `.github/agents/accessibility.md`。
3. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

4. 浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，聊天会话将采用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以开始新的对话。不同练习使用的模式和模型可能不同，相关步骤会在适用时说明。

1. 如果 Chat 视图的代理下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的代理下拉菜单（它会显示当前使用的代理，默认值为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并修改项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审查。
6. 随着编辑应用，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，它们是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队专业知识和标准整理成可重复使用的代理，更有效地引导 Copilot 完成特定类型的工作。

你了解了以下内容：

- 自定义代理的定义方式；
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]，在审查更改的同时，为同一会话添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在集成开发环境（IDE）中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
