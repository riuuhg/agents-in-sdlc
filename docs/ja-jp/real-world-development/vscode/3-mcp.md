---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードでフィルタリング機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張について学びます。
- Playwright MCP サーバーを workspace に追加します。
- Copilot にブラウザーを操作させて、フィルタリング機能をテストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部に接続するとさらに便利になります。Copilot が外部のツールやサービスと通信するための標準的な方法が、Model Context Protocol (MCP) です。

![エージェント モードがコンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) と連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを通じて最新情報にアクセスし、ツールを通じて代わりに操作を実行できます。

MCP サーバーは、AI エージェントと外部のツールやサービスの間を仲介します。既存の API や npm package のようなローカル ツールとの通信を管理し、エージェントがアクセスできるツールとリソースを提供します。

よく使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、issue、プルリクエストの管理 API を提供します。新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などをエージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を提供します。Web ページの表示、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、エコシステム内の MCP を見つけて貢献できる [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。利用前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセス許可に注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code workspace で利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` サーバーがすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` 設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot が issue のバックログを読めました。これは [remote GitHub MCP server][remote-github-mcp-server] を使います。ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` で `github` の隣に `playwright` を追加し、ファイルを次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面にウィンドウを表示せずにブラウザーを実行します。デスクトップ環境がない Codespace では、このオプションが必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使うため、通常はブラウザーがすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うときに起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか求められたら、構成を確認して信頼を選択し、起動を許可します。

手動で起動する必要はありません。次の手順で Copilot が初めてツールを使うときに、VS Code が起動して信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。自分でアプリを操作する代わりに、エージェントにページを開かせて移動し、フィルターを適用し、結果を読み上げてもらえます。会話を離れずに機能を確認できる手早い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目などのラベル付き要素を基に判断します。支援技術と同様の構造を使うため、機能確認と簡単なアクセシビリティ確認を同時に行えます。

サーバーを接続してアプリを起動した状態で、Copilot にフィルタリング機能を確認するよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を確認して結果を報告します。issue の受け入れ条件に照らして報告を読みます。問題がある場合は、プルリクエストを作成する前に質問するか、コードを修正するよう依頼します。

> [!NOTE]
> テスト時にアプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームがレビューできるよう PR を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit message を生成します。

   ![Copilot が行った変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub command-line interface (CLI) などで PR を作成できます。GitHub Copilot を使って、関連 issue を検索し、PR を関連付けてもらいましょう。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. agent dropdown で **Agent** が選択され、Copilot が GitHub ツールを使えることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を集めて操作することを許可します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。画面の指示に従ってください。
5. Copilot が issue を検索して該当の issue を見つけ、PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張について学びました。
- Playwright MCP サーバーを workspace に追加しました。
- ブラウザーでフィルタリング機能を手動でテストしてからリリースの準備をしました。
- ブランチを公開し、フィルタリング作業の PR を作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどに使える[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
