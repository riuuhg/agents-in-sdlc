---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- custom instruction でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリング PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使う slash command を確認しました。

## リリースした内容

このワークショップには、それぞれ最新の `main` から作成したブランチで進める 2 つの PR があります。

1. **星評価**: ゲーム カードに既存の `starRating` と、評価がない場合の状態を表示します。
2. **フィルタリングと品質確認のワークフロー**: フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルタリングを計画してから PR を作成するまで、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作でコードを確認しました。`quality-checks` skill は、新しい形式で単体テスト、lint、型チェックを実行して結果を報告しました。QA は、その結果と要件、テスト範囲のレビューをまとめて PR の前に確認しました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果を確認します。変更後は、影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームの必要に応じて調整できます。instruction は標準を示し、skill は繰り返し行う作業を記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更を一緒に進めるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選ぶとよいでしょう。

ツールと同様に、コンテキストも重要です。何を作りたいか、なぜ必要か、どのように進めたいかを明確に伝えると、結果が大きく変わります。

## さらに試す

中心的なワークフローを完了しました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して進めます。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達するには使い続けることが一番です。本番コードや趣味のコード、何年も作ろうと思いながら実現できていない小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の作業環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
