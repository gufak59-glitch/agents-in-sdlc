---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を定義すると、Copilot は特定の種類の作業をより効果的に行えます。チームの専門知識を再利用できるエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを実践するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown のプロンプトを記述します。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。最も分かりやすい違いは、**カスタムエージェントが作業者で、スキルがツール**だということです。

カスタムエージェントは専用のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを使うよう調整できます。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフローやテストの実行と管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせてテストと改善を繰り返してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。Copilot をどのように導くか理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します（演習 3 の絞り込み PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続けて次の項目を確認します。
   - アクセシブルな Web サイトのコード生成を担う中心的な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント一覧には、`.github/agents` にあるすべてのカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント一覧から **Agent** を選択します（すでに選択されている場合はそのまま進みます）。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント一覧（アクティブなエージェントが表示されます。既定では **default**）を選択します。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。編集内容は、ファイル パスとレビュー可能な差分とともにチャットに表示されます。
6. エージェントがファイルを変更すると、**Files changed** の表示が更新され、変更されたファイルを確認できます。

> [!NOTE]
> この作業には数分かかります。Copilot はリポジトリに変更を加え、Astro コンポーネントや CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューして方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]を学びました。カスタムエージェントを使うと、チームの専門知識と標準を再利用できる形でまとめ、Copilot をより効果的に導けます。

次の概念を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェント モードでカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して方向を調整します][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
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
