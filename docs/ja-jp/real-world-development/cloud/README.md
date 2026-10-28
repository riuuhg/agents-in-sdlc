---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成します。その間、別の作業を進められます。

このワークショップでは、まず cloud agent が従うカスタム instruction を追加し、GitHub issue を割り当てて実装を依頼します。カスタムエージェントをレビューして活用し、agents ダッシュボードからセッションを監視して方向付けます。最後に、cloud agent が作成したプルリクエストをレビューし、結果を反復して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム instruction][ex1] | コンテキスト | cloud agent が従うカスタム instruction を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して方向付ける |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外のすべてのプラン (**Copilot Student、Pro、Pro+、Business、Enterprise**) で利用できます。Copilot Business または Copilot Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このコースでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI コース](../cli/)または [VS Code コース](../vscode/)を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
