---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instruction][previous-lesson] |
|:--|

多くの組織が、未解決のセキュリティ問題、更新が必要な古いコード、実装する時間がなくバックログに残った機能など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた issue の重要性を学ぶ。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys では、解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを残しておらず、ほとんどのコードに説明がありません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲーム詳細ページに、同じカテゴリに属する関連ゲームを表示し、ユーザーがほかのゲームも見られるようにします。まずは受け入れテストで UX を試せる程度の実装が必要です。この作業はブロッカーですが、ほかに優先度の高い issue があります。

優先度が下がりやすいこのようなタスクは、Copilot cloud agent に任せるのに適しています。エージェントに非同期で作業させる間、開発者は別のタスクに取り組み、後から作業結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てられると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取り組みます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした instruction の重要性

GitHub Copilot に魔法のような力はなく、考える必要がなくなるわけではありません。一見簡単な作業でも、詳細を確認すると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では、custom instruction と Copilot への指示方法を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、cloud agent に作業を割り当てる前に、このリポジトリの instruction file を読み、後でその影響を確認できるようにしましょう。

リポジトリの GitHub Web UI または Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認し、TypeScript 規約と TSDoc doc comment の要件に注目します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに instruction を適用していることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てます。プルリクエストに含まれる TSDoc doc comment、TypeScript 規約、コメント ヘッダーを確認してください。これらの内容は instruction file によるものです。後の演習でも、PR のレビュー時に確認します。

## Copilot cloud agent の開発環境を準備する

コードの作成には、必要な開発ツールや依存関係をインストールする環境設定が必要な場合があります。Copilot にタスクを割り当てるときも同様で、cloud agent はソフトウェア エンジニアのように作業します。

Cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップ ワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係を用意できます。ラボの流れをスムーズにするため、プロジェクトでは事前に設定されています。単一 Astro アプリの作業に必要な Node.js、プロジェクト依存関係、E2E テスト用のブラウザー、移行と seed を済ませた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要最小限の権限を設定します。Copilot は操作用の独自 token を使います。
    permissions:
      # セットアップでリポジトリを clone して依存関係をインストールする場合は contents: read が必要です。clone しない場合は、手順の完了後に Copilot が自動で clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Node.js を使うフロントエンド / アプリのセットアップ (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースを migrate して seed し、build とテストにデータを用意します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub workflow と同様の形式ですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つの job があります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` が指定されているため、リポジトリの **Actions** タブから手動で実行できます。Copilot の実行を待たずに workflow の成功をテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは広く知られていますが、情報が古い、あるいはまったくないプロジェクトもあります。開発の生産性を下げ、コードベースの保守や新しい開発者の参加を難しくする技術的負債です。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した issue です。エージェントにバックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com の自分のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しいプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo list を作成します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前に説明した workflow を実行)、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. todo list を確認し、Copilot が実行するタスクを把握します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** を選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が出るまで、ブラウザー ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、方針を考えてコードを作成する様子が表示されます。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は、非同期に作業を実行し、その間に別の作業ができることです。別のタスクを割り当てた後、アプリケーションに機能を追加するコード作業を行います。

## ゲーム詳細ページに関連ゲームを追加する

Cloud agent の利点の 1 つは、作業を分けられることです。デザインチーム向けの関連ゲーム表示を追加するのに時間がかからない場合でも、その時間を別の作業に使えます。Copilot cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成画面を開きます。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![Copilot が issue に対応中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。cloud agent はソフトウェア エンジニアのように動作するため、作業中に常に監視する必要はなく、完了後にレビューできます。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使うと、issue を割り当てて非同期に作業を依頼できます。技術的負債の解消、新機能の追加、別の framework へのコード移行などに活用できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた issue の重要性を学びました。
- issue を Copilot に割り当てました。

Cloud agent が作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も使用でき、前のモジュールで確認した custom instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
