---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、明確な QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが instruction、skill、MCP ツールを組み合わせる方法を確認します。
- 再利用可能な quality assurance プロファイルを作成して確認します。
- QA エージェントを選び、フィルタリングの issue に照らして報告をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタムエージェントに QA プロセスを管理させることで、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する、Copilot の専門的な役割です。プロファイルには、エージェントの目的、instruction、使用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチームの標準を示します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に伝えます。既存の機能を置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、実行するチェックと守るべき制約の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約をプロファイルに反映したことを確認します。確認しておくことで、検証だけを依頼したのに不完全または広範なエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略可能ですが、設定するとエージェント名が分かりやすくなります。
3. プロファイルの instruction を読み、QA が要件を起点とし、リポジトリ instruction に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. QA が根拠を添えて報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. 必要な責務や制約が抜けていたら、続ける前に既定のエージェントへ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開して QA を選択すると、すでに読み込んでいる issue と計画を利用できます。

1. 次のコマンドでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが instruction、skill、MCP ツールを組み合わせる方法を確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの issue に対する結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントの作成][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
