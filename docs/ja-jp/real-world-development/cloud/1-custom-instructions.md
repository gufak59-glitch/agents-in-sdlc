---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。作業に特定の手順が必要な場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにしましょう。[指示ファイル][instruction-files]を使うと、Copilot は依頼内容だけでなく、望ましい進め方も理解できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期で動作するため、リアルタイムで並べて効果を示すのは困難です。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って Copilot に適切な情報を提供し、こうした開発手法に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや希望する方法を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。関連性の高い提案やコード スニペットを得るために、好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定のファイルを対象にした指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**依頼とともに Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベスト プラクティスなど、Copilot Chat や CLI へのほとんどの依頼に関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイルの種類を対象に作成できます。TypeScript などの言語、UI コンポーネントやユニットテストの作成などの作業に関する指示を記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格に基づく指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することはこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な構成例が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、作成中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ層などのファイル種類や特定の作業に対する指示に使います。
- 自然な言葉で、明確なガイダンスと例を記載します。コードの良い例と悪い例を示すのも効果的です。

AI の使い方に唯一の正解がないのと同じように、指示ファイルの作り方にも決まった方法はありません。試しながら、プロジェクトに合った形を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまな作業を対象にしたファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。画面ごとに操作方法は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、利用中の画面に対応するレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

少し時間を取って、このリポジトリに含まれる指示ファイルを読んでください。基本となる `copilot-instructions.md` と、さまざまな作業を対象にした `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象のファイルを指定する glob が記述されています。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。パスはリポジトリのルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 か所だけです。コード内に TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーを記載するルールを追加します。後続の演習で cloud agent が issue に取り組むとき、このルールを参照します。

編集する前に、作業用のブランチを作成します（cloud agent は issue の対象ブランチにある指示を読みますが、変更を整理してコミットできるようにします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、コーディング標準にドキュメント要件を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在のコーディング標準は記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 次の Markdown を既存の標準の直後に追加し、ファイル ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体的な指示に、`*.instructions.md` は言語、ファイルの種類、特定の作業に関する指示に使えます。今回追加するようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチから指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示を参照できるようにするには、変更を `main` に反映してください。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次のハーネスで issue に割り当てたとき、cloud agent はこの指示を参照します。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスで cloud agent が作成するプルリクエストを確認するときに、追加した標準が適用されているか確かめましょう。

次は、Copilot cloud agent に最初の issue を[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
