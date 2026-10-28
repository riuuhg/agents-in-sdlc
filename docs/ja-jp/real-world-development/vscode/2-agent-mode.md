---
title: "演習 2 - Copilot エージェント モードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題を修正します。変更漏れが見つかれば、再びファイルを更新することもあります。Copilot エージェント モードを使うと、IDE の中でより自律的に作業できます。プロジェクトを調べて必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる体験を提供します。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent は issue とプルリクエストを通じてやり取りし、チームの一員のように非同期で作業します。

この演習では、次のことを学びます。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを見つけ、複数のファイルを連携して変更する方法。
- UI とデータ レイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では publishers helper を追加しました。次は、Copilot エージェント モードを使ってデータ レイヤー、UI、テストの残りを完成させます。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリケーションはビルド時に静的 HTML を生成します。Drizzle ORM を使って、各ページの frontmatter からローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` に置き、インタラクティブな動作には標準の DOM API を使う小さな Astro `<script>` を使います。

### Web サイトを起動する

`npm run dev` を実行すると、単一の Astro 開発サーバーが起動します。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後で `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace で新しいターミナルを開きます。
3. フィルタリング用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、現在の機能を確認します。

- **Home Page**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

Web サイトは動作していますが、新しい機能を追加します。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に用意されます。GitHub Copilot にバックログを表示してもらいましょう。

1. Chat ビューの agent dropdown で **Agent** を選びます。**Agent** はファイルを調べて変更を計画、実装し、ターミナル コマンドやツールを実行できます。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案をできるだけ一貫させることが目的ですが、LLM は確率的に動作するため、説明と異なる提案が表示されても正常です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP command を使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログを表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示する command の実行を許可するため、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot はユーザーに最も役立つと考えられる項目に基づいて優先順位も付けます。

## Instruction file を確認する

コードの生成を始める前に、Copilot にコンテキストを渡すための instruction file を確認します。Web サイトの機能追加方法に関するガイダンスを含む [user interface (UI)][ui-instructions] file を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - component の設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル種別に関する instruction file へのリンク。

> [!TIP]
> Instruction file から、ほかの instruction file やプロジェクトのファイルを参照できます。パスは instruction file の場所からの相対パスです。これにより、再利用や分割が容易になり、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングの完成には、少なくとも 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

コードベースにマージする前にテストも実行し、成功させる必要があります。Copilot エージェント モードにこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、instruction file を追加します。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent dropdown で **Agent** が選択されていることを確認します。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データ レイヤーの helper、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行います。タスクの複雑さやコードベースの規模によっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はなく、完了後に変更ごとに **Keep** または **Undo** を選択できます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、テストが失敗したりすることがあります。Copilot はコード生成とテストを繰り返し、エラーがなくなるまで進めます。

   ![Copilot エージェント モードの会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加しました。次の方法を学びました。

- UI とデータ レイヤーにまたがる新機能を実装する。
- プロジェクトを調べ、関係するファイルを見つけ、連携して変更する。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 発展演習 - ページ送りを実装する

ゲーム一覧が増えると、ページ送りが必要になります。この演習で学んだ方法を使い、Copilot にページ送りを追加するよう依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更やコンテキストを伝えながら、反復して作業します。これが Copilot を使う通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モードの違いと使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
