---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监控和管理智能体][previous-lesson] |
| :-- |

## 审查工作

整个练习系列中，您与 GitHub Copilot 协作完成了多项旨在改善用户体验的任务。您使用智能体模式跨客户端和服务器添加筛选功能，通过 Playwright MCP 服务器在浏览器中手动测试，然后使用自定义智能体实现无障碍功能（高对比度和浅色模式切换开关），并在会话进行过程中引导它扩展工作。现在，将本地工作发布，并像团队成员一样审查。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。Copilot 生成的内容仍然需要经过正式审查。现在从 codespace 推送无障碍更改、创建拉取请求，并在让团队其他成员参与之前审查差异。

## 发布无障碍功能

您在[练习 4][exercise-4]和[练习 5][exercise-5]中使用无障碍自定义智能体实现的高对比度和浅色模式切换开关，已作为已提交更改保存在 codespace 中。现在将其推送到分支并创建拉取请求，让团队成员审查。

1. 返回 codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍更改已提交。如果练习 5 中仍有未提交的更改，请暂存并提交，使用能准确描述更改的消息，例如 `Add high-contrast and light-mode toggles`。
4. 选择 **Publish Branch** 发布分支（也可以使用 **...** 菜单 → **Push**）。
5. VS Code 会提示在 github.com 上打开新分支。接受提示，或手动打开存储库并在分支横幅中选择 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并用简短说明总结完成的工作及其原因。
7. 选择 **Create pull request**。
8. PR 创建后，选择 **Files changed** 选项卡，全面审查工作。重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
9. 返回 **Conversation** 选项卡。
10. 如果有工作流等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。若一切正常，工作流应会通过。

> [!TIP]
> 希望获得对无障碍工作的第二种意见？可以在 PR 评论中提及 `@copilot`，并提出类似“审查此 PR 是否存在其他 WCAG 问题”或“建议改进键盘导航”的请求。Copilot 会启动新会话处理评论。

## 可选练习 - 继续本地探索

在 IDE 中与智能体迭代协作是一项需要反复练习的技能。可以在 VS Code 中开启以下后续会话：

- 在游戏详情页添加支持者意向表单。
- 为游戏列表页实现分页。
- 为 `src/lib/` 中的数据访问辅助函数添加输入验证和错误处理。
- 扩大无障碍智能体的职责范围，例如审查整个网站中的键盘焦点顺序。

## 总结

恭喜！您已完成 VS Code 练习系列。本次练习中，您：

- **使用 Playwright MCP 手动测试功能。**添加 Playwright MCP 服务器，并让 Copilot 操作浏览器，在创建拉取请求前验证筛选功能。
- **使用智能体模式协调跨层更改。**在同一会话中添加筛选功能，涉及客户端、服务器和测试。
- **使用自定义智能体。**在智能体选择器中选择无障碍自定义智能体，并观察它如何根据存储库要求实现高对比度模式。
- **管理并引导智能体会话。**以内联方式审查提议的更改、接受需要的内容，并通过浅色模式后续请求扩展会话。
- **通过拉取请求完成工作流程。**发布本地更改，并像团队成员一样全面审查。

## 回顾和后续步骤

本课结束了必修的 VS Code 练习系列。到此即可完成本次研讨会。

如果想拓展对 Copilot 智能体能力的了解，其他练习系列会通过不同界面介绍相关场景：

- 💻 **[CLI 练习系列](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、技能、自定义 QA 智能体、Playwright MCP、Agent Merge 和上下文感知交接。
- ☁️ **[云智能体练习系列](../../cloud/)** — 学习分配议题、通过智能体页面监控会话，以及异步迭代改进拉取请求。

还可以继续完善刚刚开始的工作。[awesome-copilot][awesome-copilot]汇集了更多可调整并用于自己项目的说明文件、自定义智能体和技能。

如果想选修扩展内容，[可选：集成 Foundry][exercise-7]会使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge 并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
