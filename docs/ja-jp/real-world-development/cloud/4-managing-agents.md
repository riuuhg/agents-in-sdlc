---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。作業内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- agents ページから cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを依頼した後、チームはライト モードも追加するとよいと気付きました。サイトのスタイル更新と切り替え機能の実装が進んでいるため、同時に追加するのが合理的です。エージェントがハイ コントラストに加えてライト モードも追加するよう、作業の方向を伝えます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方にタスクが表示されます。Copilot に割り当てた次のようなタスクが一覧にあることを確認します。
   - コードのドキュメントの更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイ コントラスト モードの追加。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれます。
   - リポジトリの checkout。
   - Copilot の作業環境の作成。
   - MCP サーバーの設定。
   - 割り当てられたタスクを完了するための作業。

> [!NOTE]
> 表示される手順は、Copilot の進行状況や作業方法によって異なります。

4. 右側の PR ペインも確認します。PR と変更されたファイルが表示され、さらに状況を監視できます。

## Cloud agent に作業の方向を伝える

実行中のタスクを確認できたので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの作業に関するセッションを選択します。タイトルは Copilot が付けた名前や作業の状態によって異なります。

   ![Mission control でアクセシビリティ セッションを表示する画面](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents ページで、作業中の cloud agent に指示するダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に追加することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼をワークフローに加えます。

今回も数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して方向付ける中心的な場所である Copilot agents ページを確認しました。進行中や完了済みのタスクを追跡し、実行中の作業をレビューし、追加の指示を送れます。

次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
