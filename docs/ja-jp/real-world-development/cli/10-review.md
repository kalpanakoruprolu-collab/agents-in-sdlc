---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されていたフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードで確認しました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズし、ユニット テスト、lint、型チェックに使いました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルター機能の変更全体を確認し、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新した `main` から作成したブランチで進める 2 つの PR があります。

1. **星評価:** 既存の `starRating` と未評価の状態をゲーム カードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを含めます。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は PR の前に、結果と要件およびテスト カバレッジの確認をまとめました。

追加するテストは実際の不足を補うものでなければなりません。QA を実行しても新しいテストが不要な場合、それが正しいこともあります。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業の質を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整します。命令は標準を設定し、スキルは繰り返し可能なタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に進め方を考えるには **Plan**、焦点を絞った変更に参加し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

コンテキストは、インフラストラクチャと同じくらい重要です。何を、なぜ、どのように作るのかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを終えました。次の CLI 機能も確認できます。

- `/review` でコード レビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールを使い続けることが、上達への一番の近道です。プロダクション コードや個人のプロジェクト、長年作ろうと思っていた小さなアプリにも使ってみましょう。チームに学んだことを共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリ ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

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
