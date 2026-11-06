---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。作業に決まった手順がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストにアクセスできるようにしましょう。[指示ファイル][instruction-files]を使うと、コード生成の指針を Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、絞り込み機能の最初のデータ部分（パブリッシャー ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が更新したガイダンスを使えるよう、指示の変更と絞り込み機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成されたコードは、設定した標準の一部に従わないことがあります。Copilot の動作は確率的です。この演習の目的は、指示の更新後に動作がどのように*変化する傾向にあるか*を確認することであり、出力の文字列を完全に一致させることではありません。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**依頼とともに Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容、ベスト プラクティスなど、ほとんどの Copilot Chat や CLI の依頼に関係する情報を記載します。
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
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** の指示も含め、これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、適用対象のファイルを指定する glob が記述されています。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分割し、参考例を Copilot に示せます。パスはリポジトリのルートではなく、指示ファイルの場所を基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前に** Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを変更します。ヘルパー ファイルとテストが変更されることもあります。
6. データ層の規約を適用する `.github/instructions/drizzle.instructions.md`（`src/lib/*.ts` に適用）に従い、生成されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数になっていることを確認します。
7. 生成されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。指示を更新した後に動作が*一貫すること*が重要です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、コーディング標準にドキュメント要件を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在のコーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 次の Markdown を既存の標準の直後に追加し、ファイル ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体的な指示に、`*.instructions.md` は言語、ファイルの種類、特定の作業に関する指示に使えます。今回追加するようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットする

指示ファイルもリポジトリの他のファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。手動で作成することもできますが、接続済みの GitHub ツールを使って Copilot に issue を検索させ、issue をクローズする PR を作成してマージを依頼します。

> [!NOTE]
> Copilot に GitHub 上の作業を依頼するのはこの演習が初めてです。プロジェクト テンプレートでは GitHub のツールがワークスペースに設定済みなので、追加のセットアップは必要ありません。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインが求められることがあります。画面の指示に従って許可してください。後の演習で、この接続の仕組みである Model Context Protocol を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、次の絞り込み作業に向けたパブリッシャー ヘルパーの基盤の両方を説明するようにします。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索し、PR の作成を開始します。
5. Copilot が GitHub 操作を実行する許可を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、PR をマージしてローカル ブランチを `main` に戻すよう次のプロンプトで依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェル コマンドを実行する許可を求めたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 現在の指示でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準を採用する様子を確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェント モードで指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
