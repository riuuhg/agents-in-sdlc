---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉基本操作，并找到预置的筛选议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一款功能强大的代理式编码助手，可在终端中运行，让您通过命令行探索代码库、生成代码、运行命令并与外部工具交互。您可以将任务交给它、请求修改，同时专注于当前工作。第一步当然是安装这个工具！好在可以使用您熟悉的工具来完成安装。

本课将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成身份验证。
- 信任研讨会存储库并尝试一次简短对话。
- 通过内置的 GitHub MCP 服务器查找筛选议题。

## 场景

您的团队开始使用 AI 代理来处理日益增长的待办事项。Copilot CLI 将这种能力带到终端中，而许多开发者平时就在终端中工作。本课将完成安装和身份验证，为您在研讨会其余部分使用它做好准备。

## 安装 Copilot CLI

您可以通过 [npm][install-cli]、WinGet 和 Homebrew 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本课将使用 npm。

1. 返回 Codespace 并打开终端。
2. 验证已安装 Node.js，且版本符合要求：

   ```bash
   node --version
   ```

   您应该会看到 24 或更高版本。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   您应该会看到版本号。

## 使用 GitHub 账户完成身份验证

首次启动时，Copilot CLI 会提示您使用 GitHub 账户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果系统提示，请按照设备代码说明进行身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认路径指向您的 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**。

> [!NOTE]
> 在 Codespace 中，您可能已经通过 GitHub 会话完成身份验证。如果启动 Copilot CLI 时没有提示身份验证，就可以继续使用。

## 熟悉基本操作

在普通 Shell 提示符下输入的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给代理，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看已安装版本提供的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回答，留意 Copilot 如何先探索存储库再作答。
5. 输入 `/mcp list`，确认内置的 GitHub MCP 服务器可用。
6. 请 Copilot 查找筛选议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开该 URL 并阅读议题。完成一次快速的首次修改后，您会用到它。

> [!TIP]
> 普通的 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次修改前，您都会创建一个功能分支。

## 使用研讨会快捷方式

Copilot CLI 通常会在使用已获准权限范围之外的工具之前征求许可。在本研讨会中，您将使用 `--yolo` 这个经您同意的快捷方式重新启动它，从而在 Codespace 中省去这些确认提示，专注于练习。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。请仅在 Codespace 或虚拟机等隔离环境中使用，切勿将其设为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

在本研讨会中，`--enable-all-github-mcp-tools` 会启用后续课程中用于处理议题和拉取请求的 GitHub MCP 读写工具。Codespace 限制了对本地计算机的访问，但经过身份验证的 GitHub 资源仍是真实资源。发布或合并前，请检查所有更改。

1. 使用 `/exit` 退出 Copilot CLI。
2. 在存储库根目录中重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再询问一个关于项目的简单问题，确认对话正常，然后使用 `/exit` 退出。

Copilot 会自动保存对话。之后更改说明或添加代理时，您将使用 `copilot --resume` 返回同一个功能会话和分支。

## 总结和后续步骤

恭喜！本课中，您：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 账户完成了身份验证。
- 信任了研讨会存储库并尝试了一次简短对话。
- 通过内置的 GitHub MCP 服务器找到了筛选议题。

接下来，您将[开始首次专注的修改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
