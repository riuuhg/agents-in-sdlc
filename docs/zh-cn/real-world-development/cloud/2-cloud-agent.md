---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置在待办事项中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，智能体会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 了解如何安全地执行操作。
- 认识范围明确的议题为何重要。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，应用程序完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望每款游戏的详情页显示相关游戏（同一类别中的其他游戏），方便用户继续浏览。团队暂时不需要完善的实现，只需要可用于 UX 验收测试的版本。此工作目前受阻，但有其他优先级更高的议题。

这些任务很容易被推迟，因此适合分配给 Copilot 云智能体。它可以异步处理任务，开发者则可专注于其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像人类开发者一样在后台执行任务。与人类同事协作的方式类似，可以通过多种方式开始工作，其中一种是[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。您可以在任务进行中或完成后查看 Copilot 会话。提议的解决方案准备好供审查时，Copilot 会在拉取请求中通知您。

## 明确限定任务范围的重要性

GitHub Copilot 并非魔法工具，使用它并不能省去思考。即使看似简单的操作，也可能包含复杂细节，因此[为 Copilot 云智能体明确限定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。采用生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明实操，就已经看到 `.github/copilot-instructions.md` 的作用；如果尚未完成，现在可以快速查看。分配任务前，先只读查看存储库中已有的说明文件，以便之后识别它们的影响。

在存储库的 GitHub Web 界面中，或在已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将这些说明限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释标头；这些要求来自上述说明文件。后续练习审查 PR 时还会再次提到。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境，并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 时也一样，因为它的工作方式类似软件工程师。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在智能体开始工作前运行，以便安装所需的开发工具和依赖项。本练习已预先配置此工作流。它会为单体 Astro 应用安装 Node.js 和项目依赖项、端到端测试所需的浏览器二进制文件，以及经过迁移并初始化数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许您在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在采用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并初始化本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许您在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

所有人都知道文档的重要性，但多数项目的信息已经过时或完全缺失。这类技术债务很容易被忽视，降低工作效率，也让维护代码库或引入新开发者变得更加困难。Copilot 擅长生成文档，因此非常适合将这类任务交给云智能体。它会在后台生成所需文档，之后可在后续练习中审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR。标题可能类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表，因为它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方式。

13. 查看 Copilot 将要执行的任务。
14. 向下滚动 PR 时间线，应会看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解状态、暂停思考并选择合适计划，以及创建代码。

这可能需要几分钟。云智能体的主要优势之一是能够异步执行任务，让我们专注于其他工作。接下来利用这一点：先将另一个任务分配给云智能体，再转而编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏部分

Copilot 云智能体的一大优势是可以拆分工作：您专注于一组任务，而它处理另一组。即使为设计团队添加相关游戏部分不会花费太久，这段时间仍可用于其他工作。现在将这项任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，您会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正认真处理第二项请求。云智能体的工作方式类似软件工程师，因此无需主动监控；等它完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结和后续步骤

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课中，您：

- 自定义了代码生成环境。
- 了解了如何安全地执行操作。
- 认识到清楚限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作。接下来，我们将创建并使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
