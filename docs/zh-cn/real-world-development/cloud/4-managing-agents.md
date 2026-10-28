---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，您要求 Copilot 云智能体处理了三个分别旨在改善用户体验和添加功能的任务。云智能体专为异步自主工作而设计，但监控这些任务仍然很重要。

GitHub 提供了多种工具来管理分配给云智能体的任务，包括 [GitHub.com 上的智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含未关闭拉取请求 (PR) 的智能体任务，也可以探索智能体执行的操作，甚至引导正在进行的会话。

本课将：

- 探索智能体页面，监控云智能体任务。
- 在会话进行过程中引导智能体，要求其添加其他功能。

## 场景

将高对比度模式分配给智能体后，团队意识到也可以添加浅色模式。既然正在更新网站样式并添加切换功能，就适合同时实现此功能。您将引导智能体，使其在添加高对比度模式的同时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 当前的工作状态和采用的方法而变化。

4. 注意右侧显示的 PR 窗格。在此可以查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

您已查看正在执行的任务。现在要求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择名称中提到添加高对比度模式的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现类似下方的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中高亮显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，再将新请求纳入工作流程。

这同样需要几分钟，可以借此暂停并回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过这个任务控制中心，您可以跟踪所有正在进行和已完成的任务、审查已执行的工作，还可以重定向正在运行的任务以调整范围或提供额外指导。

本课中，您：

- 探索了任务控制中心和智能体页面，监控云智能体任务。
- 重定向了正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
