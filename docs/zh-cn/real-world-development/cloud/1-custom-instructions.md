---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可提供此类指导，帮助 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将：

- 了解项目专属上下文、编码规范和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习系列不同，这里不会运行更改前后的提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查云智能体生成的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。它可以指导 Copilot 提供更符合预期的建议和代码片段。您可以指定首选的编码约定、库以及注释风格。说明既可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个存储库级说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。例如，可以为 TypeScript 或 Astro 编写说明，也可以为创建 UI 组件或一组新测试编写说明。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级说明，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [支持使用其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

关于创建说明文件的完整讨论超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供专门说明。
- 使用自然语言，确保指导清晰，并提供代码示例说明应采用和避免的写法。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践，您会逐渐找到适合项目的做法。

> [!TIP]
> 使用 GitHub Copilot 的每个项目都应建立完善的说明文件集合。探索本项目中的说明文件时，可能会发现有些文件面向多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。与当前产品界面相关的课程会介绍相应操作。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

花些时间查看本存储库附带的说明文件：其中有一个核心 `copilot-instructions.md`，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 子部分。这些内容适用于您与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。可以看到针对 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob，用于确定适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 查看本项目中专门针对单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。相关路径以说明文件所在目录为基准，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求提供代码内文档。接下来会添加 TSDoc 文档注释和文件注释标头的规则。

## 添加新的存储库标准

接下来要做的是：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释标头。在本练习系列后续部分处理分配给云智能体的议题时，它会遵循这条规则。

编辑前，先创建并切换到新分支（云智能体会读取议题目标分支中的说明，但您仍应通过提交妥善记录更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加项目编码标准，以便改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。查看其中记录的项目编码标准；目前尚无代码内文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指导，也可以为特定语言、文件类型或任务创建 `*.instructions.md` 文件。存储库级文件适合放置您刚添加的文档注释规则等项目标准。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。在下一课将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。这样，云智能体就能在下一课处理议题时读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，也可以跳过分支步骤并直接提交到 `main`。创建分支是为了让研讨会更贴近真实项目中的工作方式。

## 总结和后续步骤

您已了解 Copilot 如何使用本项目的说明文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。接下来几课中，您可以在云智能体生成的拉取请求中看到该标准的应用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 响应的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更有效 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
