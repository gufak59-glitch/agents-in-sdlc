---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更し、テストを実行して、デバッグし、変更漏れのファイルを見つけ、さらに更新する、という作業を繰り返します。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE 内でより自律的に作業します。開発者と同様に、プロジェクトの構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる利用方法を想定しています。エージェント モードは IDE 内で作業し、素早くやり取りしながら進めます。cloud agent はチームのメンバーのように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する方法。
- GitHub Copilot エージェント モードが UI とデータ層にまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。残りのデータ層、UI、テストを Copilot エージェント モードで仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため Tailspin Toys の Web サイトを確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、その詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは Drizzle ORM を通じてフロントマターからローカル SQLite データベースを直接照会します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、必要な操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Codespace 上で Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトは動作していますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント一覧から **Agent** を選択します。エージェント モードは、ファイルにまたがる変更を計画、実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨する意図はありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。モデルをそろえることで、体験を安定させることを意図しています。ただし LLM の動作は確率的なため、ワークショップの例とは異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の動作は確率的なため、Copilot が異なる MCP コマンドを使用することがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと考えた項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトに機能を追加する方法が記載された [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類に応じた指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準とします。これにより、指示を再利用し、複雑な内容を管理しやすい小さな部分に分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) に残りの絞り込みロジックを追加または改善する。
- 絞り込みの動作を確認するテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

コードベースにマージする前に、テストを実行して成功させる必要もあります。Copilot エージェント モードに作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルの選択例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探します。データ層のヘルパー、UI、テストを確認して、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては数分かかることがあります。その間、エディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot に促されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了してエラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードのチャット セッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに新機能を追加しました。

- GitHub Copilot エージェント モードで UI とデータ層にまたがる機能を実装しました。
- エージェント モードでプロジェクトを調べ、関連ファイルを特定し、変更を調整しました。
- コードベースにマージする前に、エージェント モードで生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してください。プロンプトを作るときは、次の点を検討します。

- 指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更とコンテキストを伝え、反復的に調整する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
