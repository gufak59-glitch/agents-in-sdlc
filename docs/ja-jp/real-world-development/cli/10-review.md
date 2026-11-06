---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考情報を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトとシード済みの絞り込み issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み issue から始め、Plan モードで方法を決め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証証拠を確認しました。
- 絞り込み機能の変更全体をレビューし、Agent Merge に PR のマージを許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作成し、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示を、ゲーム カードに追加します。
2. **絞り込みと品質ワークフロー:** 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

絞り込みの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA は要件とテスト範囲のレビューにその結果を加えて、PR の前にまとめて確認しました。

追加するテストは、実際の不足を補うものでなければなりません。追加テストが不要という QA の結果が正しい場合もあります。マージを許可する前に、コードと検証結果をレビューし、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、生成結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し作業を定義し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに応じて**モードとモデル**を選びます。実装前に方法を検討するには **Plan**、範囲の絞られた変更でやり取りを続けるには **Interactive**、要件が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。*何を*作るのか、*なぜ*作るのか、*どのように*作るのかを明確に伝えると、結果が大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

ツールを上達させる最善の方法は使い続けることです。実際のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
