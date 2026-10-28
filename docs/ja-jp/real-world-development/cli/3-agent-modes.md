---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue に基づくフィルタリング機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、適切な方法で必要な機能を構築できるように設計されています。このレッスンから数回にわたり、一般的なエージェント駆動の開発プロセスを進めます。まず issue から新機能を作り、コードと機能の動作を検証し、最後に変更をプロジェクトにマージします。

> [!NOTE]
> 機能開発の間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot との新しい会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の開発を続ける中で、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリや発行元で絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意すべき詳細があります。Plan モードで方針を決めてから、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに取り入れても、基本は変わりません。むしろ、これまで以上に重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記載した issue から始めます。
2. 何を構築するか計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れを基にしています。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューと検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行する前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントはコードを作成し、テストを実行して、タスクが完了するまで自律的に改善します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止中の場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、Copilot に作業を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、計画に時間をかけるとよいでしょう。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、実装方法を検討し、計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に基づいて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 決めた方針と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、タスクが完了したと Copilot が判断するまで、問題に取り組みながら変更を反復します。

1. **Accept plan and build on autopilot (recommended)** を選択します。インストール済みのバージョンで同様のラベルが表示されている場合は、その項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って変更を繰り返し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。必ず計画を先に確認してください。依存関係が不足している場合やポートが競合している場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画時に決めた内容と照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードに問題がないように見えても、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行元のフィルター、およびその組み合わせを試します。
4. リセット時と結果が空の場合の動作が、issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub issue から Copilot との新しい会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、コードが文書化されたプラクティスに従うよう、[カスタム instruction の使い方][next-lesson]を詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
