---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントをそれぞれ独立したワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートから用意されたバックログなど、ワークスペースを確認します。
- クイックチャットを試し、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを導入しています。Copilot app を使うと、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うため、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地が有効な GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選択する画面が表示されたらテーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに表示されている場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されていることを確認します。ここで使うレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にグループ化され、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには主に次の領域があります。

- **New** - 新しいチャットセッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合でプルリクエストを表示します。アプリを離れずに参照や絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - リポジトリの Issue を表示します。参照や絞り込み、Issue からのセッション開始を行えます。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。To-do リスト、定期的な保守、その他の手間のかかる作業を任せるのに役立ちます。振り返りでは次のステップとして紹介します。別のワークショップ演習ではありません。
- **Customize** - MCP server、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP の設定に使います。
- **Chats** - ブランチや専用ワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを始めると表示されます。表示されていない場合は **Sessions** の横の **+** を選び、最初のチャットを開始します。
- **Sessions** - リポジトリのフォルダーと、その中のエージェント セッションをまとめます。リポジトリ名 (たとえば `tailspin-toys`) を選択するとリポジトリ画面が開き、その下のセッションを選ぶと会話に戻ります。セッションはそれぞれ独立したワークスペースで実行されるため、変更が競合せず複数のセッションを同時に実行できます。最初のセッションは星評価を追加するときに開始します。

ワークショップを進めながら、ワークスペースの機能を確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能か知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

ほとんどのプロジェクトにはバックログがあり、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意された項目を確認します。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリ フォルダーを選択します。その下の個別セッションではありません。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選んで詳細を読みます。各 Issue はエージェント セッションの開始点にもなります。最初の小さな変更が終わったら、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。別のリポジトリの作業項目を見るには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問してみましょう。**クイックチャット**ではブランチや worktree を作らずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横の **+** を選択して最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションの仕組みを質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが独立した git worktree で実行されるため、変更が競合せず複数のエージェントを並行して実行できます。会話は続けることも、新しいチャットをいつでも開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールしてプロジェクトを接続し、ワークスペースを確認しました。次の方法を学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの質問をする。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使い始め方][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
