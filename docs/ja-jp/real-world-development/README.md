---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復しながら進めます。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングを提供する開発者向けテーマのサービスに取り組みます。チームのバックログは GitHub issue として用意されています。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発に使いたい環境を選んでください。各ワークショップはそれぞれセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、instruction、skill、カスタムエージェント、Playwright MCP、Agent Merge、便利な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業の方向付けや進行状況の監視を行い、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェント モード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
