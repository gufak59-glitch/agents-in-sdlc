---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で issue の絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作し、適切な方法で必要な機能を開発できます。このレッスンから数回にわたり、issue から新機能を実装し、コードの妥当性と機能の動作を確認して、プロジェクトにマージするまでの一般的なエージェント型開発の流れを実践します。

> [!NOTE]
> 機能開発を進める間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、ユーザーがカテゴリやパブリッシャーでゲームを絞り込めるようにする必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、範囲を限定して Autopilot に実装を任せます。

## 背景

AI コーディング エージェントを開発フローに取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 必要な作業を説明した issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 手順の詳細は、チームや組織によって異なります。ただし、多くの場合は上記の流れに近いものです。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで自律的に改善します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## 絞り込み機能を計画する

計画を立てると、機能の実装方法やタスクの進め方を Copilot に任せる前に決定できます。複雑な作業では、少し時間をかけて計画するのが効果的です。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、最適な方法を判断し、計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に基づいて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで問題に取り組み、反復的に改善します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐ始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了とみなす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

他のコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、絞り込み機能の実装とテストを確認します。
3. 結果を issue と計画時に決めた内容と照らし合わせます。
4. コードをレビューしたら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がなくても、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャーの絞り込み、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

このレッスンでは、会話モードを切り替えながら機能を開発してレビューしました。

- GitHub issue から Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化された規約に沿って生成されるよう、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
