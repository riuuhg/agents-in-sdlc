---
title: "演習 1 - カスタム instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[Instruction file][instruction-files] を使うと、何をしてほしいかだけでなく、どのように進めてほしいかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリの custom instruction とパス単位の instruction file を通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code コースや CLI コースとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue に対して非同期に作業するため、リアルタイムの比較が難しいためです。このコースの後半で、cloud agent が作成したプルリクエストをレビューするときに、instruction file の影響を確認します。

## Instruction file

### シナリオ

優れた開発チームと同様に、Tailspin Toys にも開発プラクティスに関するガイドラインや要件があります。たとえば、次のような内容です。

- データ レイヤーには必ず unit test を用意します。
- UI はダーク モードで、モダンな印象にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

Instruction file を使うことで、Copilot がこれらのプラクティスに沿ってタスクを実行するための情報を渡せます。

### Custom instructions

Custom instruction を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解させられます。これにより、より適切な提案やコード スニペットを得られます。好みの規約、ライブラリ、コードに含めたいコメントを指定できます。リポジトリ全体にも、特定のファイル種別にも instruction を設定できます。

Instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送られる単一の instruction file です。技術スタック、開発中のプロジェクトの概要、ベスト プラクティスなど、多くのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語別ガイドラインや、UI component や unit test を作成するためのガイダンスを提供できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの形式の instruction file][custom-instructions-support]にも対応しています。

### Instruction file を管理するためのベスト プラクティス

Instruction file の作成に関する詳しい説明は、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法を示す例が含まれています。基本的な方針は次のとおりです。

- `copilot-instructions.md` は、開発中のもの、プロジェクト構成、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスに集中させます。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなどのファイル種別や、特定のタスク向けの具体的なガイダンスに使います。
- 自然な言葉で、明確に記述します。望ましいコードと望ましくないコードの例を示します。

Instruction file の作り方や AI の使い方に唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意しましょう。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を作成してもらうこともできます。各 surface によって方法が異なります。たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` があります。現在使っている surface のレッスンで、関係する手順を確認してください。
>
> テンプレートや出発点を探している場合は、instruction file、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリに用意された instruction file を読みます。中核となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらの instruction は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があることを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、instruction の適用対象を決める glob パターン (repo root からの相対パス) を指定します。この例では、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで unit test を作成するときの instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの instruction file や、プロジェクト内のファイルへのリンクがあります。大きな instruction を小さな再利用可能な単位に分け、コード生成時に Copilot が参考にできる例を示せます。ここに記載されたパスは repo root ではなく、instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリの標準を追加する

この演習で行う編集は、コード内ドキュメントを TSDoc の doc comment とファイル レベルのコメント ヘッダーで記述する、プロジェクト全体のルールを追加することです。次の演習で issue を割り当てると、cloud agent がこのルールを読み取ります。

編集の前に作業ブランチを作成します。cloud agent は issue の対象ブランチから instruction を読み込みますが、変更をきちんとコミットしておきます。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コーディング標準を記載して、コード提案の質を高めます。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクション (27 行目付近) を探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper に doc comment が含まれていませんでした。
3. 既存の標準の直下に、次の Markdown 行を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc の doc comment を追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体のガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## instruction の更新を commit、push、マージする

Cloud agent は、issue の対象ブランチにある instruction file を読み込みます。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。cloud agent が instruction を利用できるよう、変更を `main` にマージしてください。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てる issue に取り組むとき、cloud agent はこの instruction を読み取ります。

> [!TIP]
> このワークショップで `main` を直接使う場合は、ブランチの作成を省略して `main` に直接 commit できます。実際のプロジェクトに近い手順にするため、ここではブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを得る方法を確認し、リポジトリ全体に適用する新しい標準を追加しました。残りのコースで cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction をよりよく書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
