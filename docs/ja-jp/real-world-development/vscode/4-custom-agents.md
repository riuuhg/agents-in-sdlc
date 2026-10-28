---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、目的に合わせた instruction、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用できるエージェントとして記録できます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` の YAML frontmatter があり、その後にエージェントの動作、専門知識、instruction を記載します。

### カスタムエージェントと agent skill の違い

カスタムエージェントと [agent skill][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、skill (場合によってはほかのエージェント) を連携させます。この演習のアクセシビリティ カスタムエージェントは、ガイドラインに照らしてサイトをレビューし、更新します。その際、プルリクエスト ワークフローの skill や、テストを実行、管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。テキストと背景色のコントラストが低く、現在のダーク テーマが読みづらいという意見が寄せられました。そこでデザインチームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタムエージェントに機能を実装させます。この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、あらかじめ作成されています。Copilot を導く内容を確認しましょう。

Codespace に戻り、ターミナルを開いて、フィルタリング PR とは分けてアクセシビリティ作業用の新しいブランチを `main` から作成します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` の frontmatter は、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code は `.github/agents` のカスタムエージェントをすべて Copilot Chat ビュー下部の agent dropdown に表示します。エージェントを選ぶと、会話でその instruction とツールを使えます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

1. Chat ビューの agent dropdown で、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agent dropdown (既定では **default** と表示) を選択します。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集はファイル パスと差分とともにチャット内に表示されます。
6. エージェントが変更した作業セットに合わせて、**Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかります。Copilot は Astro component、CSS、関連テストなどの既存ファイルを編集し、リポジトリに変更を加えます。

次の演習では、作業中の会話を確認して方向付けます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせて Copilot を導くカスタムエージェントについて学びました。次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェント モードでカスタムエージェントを使いました。

次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライト モードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
