---
title: "演習 1 - カスタム instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に伝えておくべき背景情報がある場合は、コンテキストを利用できるようにします。コンテキストを渡す方法はいくつかあります。特に重要なのが、コード生成のガイダンスを提供する [instruction file][instruction-files] です。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の instruction を使って、フィルタリング用 publishers helper という最初のデータ スライスを生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- instruction の更新とフィルタリングの最初の変更を `main` に commit し、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、instruction を更新した後の傾向の変化を確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

優れた開発チームと同様に、Tailspin Toys にも開発プラクティスに関するガイドラインや要件があります。たとえば、次のような内容です。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、モダンな印象にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使って、Copilot がこれらのプラクティスに沿ってタスクを行うための情報を渡します。

### Custom instructions

Custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みの規約、ライブラリ、コードに含めたいコメントを指定できます。リポジトリ全体にも、特定のファイル種別にも instruction を設定できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られます。技術スタック、開発中のプロジェクトの概要、ベスト プラクティスなど、多くのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語別ガイドラインや、UI component、unit test の作成に関するガイダンスを提供できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの形式の instruction file][custom-instructions-support]にも対応しています。

### Instruction file を管理するためのベスト プラクティス

Instruction file 作成の詳細はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` は、開発中のもの、プロジェクト構成、全体に適用する標準など、プロジェクト レベルのガイダンスに集中させます。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスク向けの具体的なガイダンスに使います。
- 自然な言葉で明確に記述し、望ましいコードと望ましくないコードの例を示します。

Instruction file の作り方や AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を作成してもらうこともできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、surface によって方法が異なります。現在使っている surface のレッスンで、関係する手順を確認してください。
>
> テンプレートや出発点を探している場合は、instruction file、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意された instruction file を読みます。中核となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらの instruction は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、instruction の適用対象を決める glob パターン (repo root からの相対パス) を指定します。この例では、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで unit test を作成するときの instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの instruction file や、プロジェクト内のファイルへのリンクがあります。大きな instruction を小さな再利用可能な単位に分け、Copilot が参考にできる例を示せます。ここに記載されたパスは repo root ではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

Custom instruction の影響を確認するため、まず現在の instruction file を使ってプロンプトを送信します。その後、instruction を更新して同じプロンプトを再送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、helper を置く場所を Copilot に示します。
3. Chat ビューの agent dropdown で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、helper とテストなどのファイルにコード変更を適用します。
6. helper が型付き関数として提案され、最初の引数に `db` client を受け取り、型付き publishers 配列を返すことを確認します。これは `.github/instructions/drizzle.instructions.md` にある `src/lib/*.ts` 向けのデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction の更新後に一貫性が向上することが重要です。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コーディング標準を記載して、コード提案の質を高めます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクション (27 行目付近) を探します。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、先ほど生成された helper に doc comment が含まれていませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体のガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が正しいファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習の土台となる実際のコードに反映できました。

## instruction を commit してブランチを push する

Instruction file もリポジトリのほかのファイルと同様に、ソース管理で管理します。変更を commit してブランチを push します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. 次のコマンドを実行し、instruction と helper の変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper の土台に対する変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。GitHub ツールがプロジェクトに接続されているため、Copilot に issue を検索させ、issue をクローズする PR を作成してマージできます。

> [!NOTE]
> この演習では、Copilot に初めて GitHub 上で作業させます。プロジェクト テンプレートには GitHub のツールがあらかじめ設定されているため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。Model Context Protocol による接続方法は、後の演習で扱います。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を検索し、現在のブランチから PR を作成して、instruction の更新と今後のフィルタリング作業の土台となる publishers helper の両方を説明するよう Copilot に依頼します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue の検索と PR の作成を始めます。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、マージしてローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作や shell command の実行を許可するよう求められた場合は、内容を確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の instruction でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が適用される様子を確認しました。
- 次の演習で続けられるよう、instruction の更新と helper の土台を `main` に commit しました。

次は、Copilot がコードベース全体に新機能を追加するときに[エージェント モードで instruction を活用します][next-lesson]。

## リソース

- [GitHub Copilot 用 custom instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction をよりよく書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
