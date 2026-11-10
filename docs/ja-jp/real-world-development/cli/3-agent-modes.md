---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue からフィルター機能を実装し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作し、適切なものを適切な方法で構築できるように設計されています。この一連のレッスンでは、issue から新機能を作り、コードを検証し、機能の動作を確認して、最終的にマージする一般的なエージェント駆動の開発プロセスに取り組みます。

> [!NOTE]
> 機能の作業は同じ会話とブランチで続けます。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため簡略化します。

このレッスンでは、次の操作を行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードを確認します。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続ける中で、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法などの詳細は、コーディング前に合意する必要があります。Plan モードで決定し、Autopilot には範囲を限定した実装を許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 作業内容を記述した issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを通るようにできます。

## 会話モード

**会話モード**はエージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: エージェントはコードの作成、テストの実行、反復を自律的に行い、タスクの完了を目指します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押し、**Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub の issue またはプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を依頼する前に機能やタスクの進め方を定義できます。複雑な作業では、計画に時間をかけることをお勧めします。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に沿って回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 決定と一致しない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成して確認したら、Autopilot で実装します。Autopilot を使うと、Copilot は完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンにある同様のオプションを選択します。
2. プロンプトの下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装を確認して検証する

生成されたコードは、ほかのコードと同様にマージ前のレビューが必要です。サイトも実行し、問題がないことを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を issue と計画時の決定に照らし合わせます。
4. 確認後、<kbd>Esc</kbd> を押して diff 表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードが良さそうでも、実行できるか確認が必要です。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、パブリッシャー、および計画で合意した組み合わせでフィルターを試します。
4. リセットと結果がない場合の動作が、issue と決定内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードを確認しました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うよう、[カスタム命令を使って][next-lesson]コード生成について詳しく確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
