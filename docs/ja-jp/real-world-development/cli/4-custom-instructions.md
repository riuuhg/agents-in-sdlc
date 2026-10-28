---
title: "レッスン 4 - カスタム instruction で Copilot を導く"
description: "リポジトリの instruction を確認してドキュメント標準を追加し、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も指定できます。フィルタリング機能を構築したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file がエージェントに適用される方法を確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

優れた開発チームと同様に、Tailspin Toys にも開発プラクティスに関するガイドラインがあります。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数は、`db` の注入可能な引数がある場合はそれも含めて、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントは `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

Instruction file を使って、Copilot がこれらのプラクティスに沿ってタスクを進めるための情報を渡します。

## Instruction file

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みの規約、ライブラリ、コードに含めたいコメントを指定できます。リポジトリ全体にも、特定のファイル種別にも instruction を設定できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られます。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの instruction 形式や対応状況は環境によって異なります。特定の形式を利用する前に、[custom instruction の対応リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには、すでに instruction file が含まれています。変更を加える前に、現在の状態を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この instruction は、このリポジトリ内の Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob パターンを指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file とプロジェクト内のファイルへのリンクを確認します。これにより、instruction を小さな再利用可能な単位に分け、Copilot に参照例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

既存のファイルにはよいガイダンスが含まれていますが、追加できる項目があります。新しく生成する TypeScript に TSDoc comment が含まれるよう、`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。フィルタリング機能のコンテキストを保ったまま新しいガイダンスを読み込めるよう、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   確認が表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc comment がコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの instruction とパス単位の instruction file がエージェントに適用される方法を確認しました。
- コーディング標準に従うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI 用 custom instruction の追加][instruction-files]
- [custom instruction の対応状況][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
