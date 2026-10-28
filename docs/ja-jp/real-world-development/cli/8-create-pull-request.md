---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング機能、instruction、skill 更新、QA プロファイル、テストをレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、quality assurance (QA) プロファイル、テストが、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は**Agent Merge**にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、instruction、skill の更新、QA エージェントを複数の PR に分けます。このワークショップでは、手順を簡潔にするため、フィルタリングと品質確認の作業を 1 つの会話とブランチにまとめ、1 つの PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を学びます。
- 機能変更全体と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の作業では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージを許可するかどうかは開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために残っている作業を自動化します。有効にすると、Copilot は CI チェックの失敗の修正、レビュー コメントへの対応、必要な場合の rebase など、PR を妨げている問題を解決します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューし、マージしてよい状態であることを確認してください。

## Agent Merge で PR を管理する

コードを作成できたので、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. コマンド `/pr create` を使って新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot が `PR #14 was squash-merged successfully.` のようなメッセージを表示することを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、不足している権限を回避しません。処理がブロックされた場合は、表示された理由を確認し、リポジトリで許可される方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルの自動化方法を学びました。
- 機能変更全体と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ[ほかの Copilot CLI slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
