---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应向 Copilot 提供相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在筛选功能已完成，你将查看 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将完成以下目标：

- 了解存储库说明和路径范围说明如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 与优秀的开发团队一样，对开发实践有一套指南和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

借助说明文件，确保 Copilot 获得必要信息，按照这些实践执行任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，帮助它了解代码风格与要求，从而生成更相关的建议。你可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型和任务创建说明。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

入门项目已包含一组说明文件。先查看已有内容，再进行更改，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。这些说明适用于你在此存储库中与 Copilot 的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看文件顶部的 `applyTo` 字段；它通过 glob 指定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并提供示例。

## 更新说明文件以符合团队指南

现有文件是不错的起点，但仍缺少一项要求。修改 `copilot-instructions.md`，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 将以下内容添加为最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的说明

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改过的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认它们准确描述代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何使用说明文件中的上下文，并将新标准应用于功能。本课中，你：

- 了解了存储库说明和路径范围说明如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 `quality-checks` 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
