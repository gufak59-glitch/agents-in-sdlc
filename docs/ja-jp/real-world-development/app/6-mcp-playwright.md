---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Customize から Playwright MCP を設定し、既存の機能用 worktree でブラウザーからフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-07-09
---

すでに説明したように、コードを書く作業には、単にコードを書く以上のことが含まれます。データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにする必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、アプリに組み込まれた機能を超えて、Copilot がさらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) の概要と、GitHub Copilot app での使用方法を確認します。
- Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。変更作業をさらに自動化し、更新が期待どおりに動作するという確信を高めるために、ユーザーと同じように Copilot が作業中の Web サイトを使えるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。これにより、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール（既存の API や NPM パッケージなどのローカル ツール）間の通信を管理します。各 MCP サーバーは、エージェントが利用できる異なるツールとリソースのセットを提供します。

よく利用されている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成や更新、issue やプルリクエストの管理などを AI エージェントに依頼できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントに依頼できます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用する前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密性の高いリソースや操作へのアクセスを許可する際は注意してください。

## Playwright MCP サーバーを追加する

サイドバーの **Customize** で MCP サーバーを管理します。リポジトリや Copilot CLI 用に設定したサーバーが app ですでに利用できる場合があるため、重複して追加する前に確認してください。利用可能な設定方法については、[app のカスタマイズ ドキュメント][customize-app]を参照してください。

1. サイドバーで **Customize** を選択します。
2. **All types** ドロップダウンを開いて **MCP** を選択し、**Installed** で既存の Playwright サーバーを確認します。
3. Playwright がまだインストールされていない場合は、利用可能なサーバーから **Playwright** を探し、**Add server** を選択します。
4. 発行元、構成、インストール時のプロンプトを確認してから承認します。プロンプトに従ってサーバーを追加してください。組織のポリシーや前提条件の不足により、セットアップがブロックされる場合があります。
5. **Interactive** モードでフィルター機能のセッションに戻り、Playwright MCP のツールが利用できることを確認します。

セットアップに失敗した場合は、続行する前に構成または権限の問題を解決してください。

## Playwright を使って機能を確認するよう Copilot に依頼する

issue と計画時に決めた内容は、すでにコンテキストに含まれています。Copilot にサーバーの起動を依頼する前に、自分で起動した開発サーバーを停止してください。

1. 次のプロンプトを使い、新しい機能を検証するよう Copilot に依頼します。

    ```plaintext
    Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
    ```

> [!NOTE]
> Copilot に特定の MCP サーバーを使うよう伝える必要はありません。通常は現在のコンテキストに基づいて適切なサーバーを選びます。ただし、重要だと分かっている情報を伝えても問題ありません。

2. Copilot の動作を確認します。

Copilot はサーバーを起動し、ブラウザーを開いて Web サイトを操作します。完了するとサーバーを停止し、結果を報告します。

## まとめと次のステップ

GitHub Copilot app から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) の概要と、GitHub Copilot app での使用方法を学びました。
- Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザー ツールを専門的な役割として組み合わせる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub Copilot app で MCP サーバーを構成する][customize-app]

[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[customize-app]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
