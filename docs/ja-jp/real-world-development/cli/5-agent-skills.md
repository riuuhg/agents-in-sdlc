---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認してレポート形式をカスタマイズし、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまでは手動でコードを確認し、instruction file を使って標準に沿うようにしてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には**agent skill**が適しています。Skill は、Copilot が適切な方法で作業を実行するためのガイダンスを提供します。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果を報告する形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys は、unit test、lint、型チェックに `quality-checks` skill を使っています。チームでは結果を読みやすくするため、レポートを改善したいと考えています。

## Instruction、script、resource

Agent skill は、再利用可能なタスクの instruction、実行可能な script、補助 resource をまとめ、エージェントが必要なときに読み込みます。基本的には、skill の名前が付いたフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、skill の名前と説明を定義する frontmatter、skill の概要、呼び出すべき状況のガイダンスを記載します。必要に応じて、実行用の script やその他の resource をサブフォルダーに含められます。

> [!NOTE]
> Skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

Skill は、チームで共有して再利用できるようプロジェクトの `.github/skills` に置くことも、ユーザー共通の `~/.copilot/skills` に置くこともできます。

## Skill を確認する

Tailspin Toys のチームが unit test、lint、型チェックを実行するために作成した `quality-checks` skill を確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明により、Copilot はいつ skill を使うか判断します。
3. instruction を読み、テストと lint の手順をどのように案内しているか確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接 skill を呼び出すことも、自然言語で依頼することもできます。3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に報告した内容を確認できるレポートにしましょう。

1. `.github/skills/quality-checks/SKILL.md` をもう一度開きます。
2. ファイルの最後に、次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

Copilot CLI は会話を再開せずに、変更された skill を再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、型チェック用の `quality-checks` skill を確認しました。
- 結果の報告形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更はフィルタリングの機能 PR に含まれます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot にサイトを直接操作させます。

## ほかの skill の例

コミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
