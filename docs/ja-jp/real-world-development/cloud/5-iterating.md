---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot と一緒にユーザー体験の改善と機能追加に取り組みました。コードにドキュメントを追加し、デザインチームのために関連ゲーム機能を実装し、ハイ コントラストとライト モードの切り替えなど、アクセシビリティ機能を追加しました。コードの変更を確認し、必要に応じて Copilot に修正を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体に変更を公開する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案をレビューします。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みは作業に使うブランチに限られます。
- Cloud agent は GitHub Actions 上で別の一時的な環境を作成して作業します。
- GitHub Actions workflow は、実行前に人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、Copilot cloud agent が作成したコード ドキュメントのプルリクエスト (PR) を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot が firewall によってブロックされているという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースに既定では制限付きでアクセスするため、これは**想定される動作**です。必要に応じて [Copilot cloud agent の firewall を設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment などのドキュメントが追加されたコードを確認します。具体的な変更内容は状況によって異なります。Exercise 2 の最初に確認した instruction file から、TSDoc doc comment、TypeScript 規約、コメント ヘッダーが反映されているかを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す表示を確認します。
8. 承認待ちがある場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションに workflow が追加されます。単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーに依頼するように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリケーションの構成を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションして、生成されたドキュメントの変更を依頼します。上記の例でも、別の依頼でも構いません。
2. **View Session** を選択し、Copilot が更新する様子を確認します。変更のための新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなくなるまで Copilot に繰り返し依頼できます。PR をレビュー可能な状態にしてから `main` にマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成コードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. workflow の承認待ちを示す表示を確認します。
7. 承認待ちがあれば **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションに workflow が追加されます。単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に `<branch-name>` を置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent に更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc doc comment を追加したりできます。この issue は instruction を更新する**前**に割り当てたため、必要に応じて追加を依頼してください。**Conversation** タブにコメントを追加すると、Copilot が新しいセッションで対応します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合は、プルリクエストに **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用の UI component。
   - ユーザー設定を保存する local storage の使い方。
   - ハイ コントラスト モードとライト モード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA label、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. workflow の承認待ちを示す表示を確認します。
8. 承認待ちがあれば **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションに workflow が追加されます。単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ハイ コントラストとライト モードの切り替えが動作し、ページを再読み込みしても設定が維持されることをブラウザーで確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベスト プラクティスに従って Copilot の実装を導いたことを確認します。問題や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 - さらに作業を委任する

Cloud agent は、バックログの実際の作業を任せ、その間に別の作業を進めるのに適しています。実践するため、リポジトリに issue を追加して Copilot に割り当ててみましょう。

- ゲーム詳細ページに、支援に関心のある人向けのフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent のコースを完了しました。次のことを行いました。

- リポジトリの custom instruction file を確認し、後で cloud agent の出力に反映されることを学びました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実装を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して方向付けました。
- Copilot のプルリクエストをレビューして反復し、**@copilot** に変更を依頼し、workflow を承認しました。

## 振り返りと次のステップ

Cloud agent のコースを完了しました。ほかのコースでは、異なる環境から関連するシナリオに取り組めます。

- 🖥️ **[VS Code コース](../../vscode/)** — IDE 内で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI コース](../../cli/)** — Plan と Autopilot、agent skill、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリで、次の作業も試せます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent の firewall を設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
