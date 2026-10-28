---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，应确保 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在您已构建筛选功能，可以探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和路径范围说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如果函数接受可注入的 `db` 参数，也要说明）。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

通过使用说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。它可以指导 Copilot 提供更符合预期的建议和代码片段。您可以指定首选的编码约定、库以及注释风格。说明既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个存储库级说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索本项目中的自定义说明文件

起始项目中已包含一组说明文件。先探索现有文件，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件中的说明适用于您与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。可以看到针对 Astro 文件、Drizzle 数据层、测试等的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定这些说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。通过这种方式，可以将较大的说明集拆分成较小且可重复使用的文件，并链接到可供 Copilot 参考的示例。

## 更新说明文件以符合团队指导

现有文件是不错的起点，但仍有缺项。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

该文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

您已了解 Copilot CLI 如何读取说明文件中的上下文，并将新的标准应用于功能代码。具体来说，您：

- 了解了存储库说明和路径范围说明文件如何传递给智能体。
- 更新说明文件以确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
