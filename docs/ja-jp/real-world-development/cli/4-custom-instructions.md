---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。作業に決まった手順がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに届く仕組みを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc / JSDoc を記載します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の仕様を記述します。関連するコードを変更した場合はコメントも更新します。
- 既存のフォーマットと lint の指示を維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コンテキストや希望する方法を Copilot に伝え、コーディング スタイルを理解しやすくできます。コーディング規約、ライブラリ、コメントの書き方などを指定して、より関連性の高い提案やコード スニペットを得られます。リポジトリ全体や、特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**依頼とともに Copilot に送信される単一の指示ファイルです。ほとんどの依頼に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、作業に対するガイダンスを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、指示ファイルがいくつか含まれています。変更を加える前に内容を確認して、影響を比べられるようにしましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで指示が適用されるファイルを指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を再利用できる小さなファイルに分割し、参考例に Copilot を誘導できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存の指示はよい出発点ですが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントを追加するため、基本の `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込ませつつ、機能のコンテキストを保つため、絞り込みの会話を終了して再開します。

1. 絞り込みの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI が会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、絞り込みの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取り込む方法を確認し、機能に新しい標準を適用しました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準を守るために指示ファイルを更新しました。
- 指示ファイルの影響を確認しました。

次は、再利用できる quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
