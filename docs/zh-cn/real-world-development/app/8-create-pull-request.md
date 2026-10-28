---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支中。现在一起审查这些更改并创建拉取请求。星级评分拉取请求 (PR) 是您手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会把功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化研讨会，我们将完整的筛选和质量工作流保留在同一个会话和分支中，并通过这个 PR 一并提交。

本课将：

- 了解 Agent Merge 以及它如何自动处理合并流程。
- 检查完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，您使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动处理剩余的 PR 工作，同时让开发者保留合并授权权。

## 了解 Agent Merge

**Agent Merge** 会在 GitHub Copilot app 中自动处理合并拉取请求所需的剩余工作。启用后，应用会话会读取 PR 并处理阻止合并的问题，例如修复失败的持续集成 (CI) 检查、回应审查意见，以及在需要时执行变基；GitHub 允许后，它会合并 PR。该流程在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，都是由您自行选择 **Merge pull request**。Agent Merge 可以代为处理这项工作，但要编辑代码并合并仍需要您的明确授权。授予合并权限前，请审查允许执行的操作和已有更改。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查后，让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅选择下拉菜单选项并不会启动流程。

Agent Merge 流程随即启动。它将：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题启动，则在说明正文中引用相关议题。
- 对目标分支执行变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控 PR 中其他开发人员或 Copilot 代码审查提供的反馈，并进行更新以解决这些意见。
- 可以选择在所有操作成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟踪进度并处理报告的阻塞项。继续前确认 PR 状态为 **Merged**；启用了合并权限或智能体仍在运行，并不代表已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前先解决这些阻塞项。

## 总结和后续步骤

您已将开发流程中的多个环节自动化，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，您：

- 了解了 Agent Merge 及其如何自动处理合并流程。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索如何与智能体协作检查、规划和可视化工作。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
