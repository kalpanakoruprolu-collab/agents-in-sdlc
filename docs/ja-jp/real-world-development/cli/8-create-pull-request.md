---
title: "レッスン 8 - 機能のプルリクエスト (PR) を作成してマージする"
description: "フィルター機能、命令、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令とスキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめて確認し、プルリクエスト (PR) を開きましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルターと品質ワークフロー全体を 1 つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証の証拠を確認します。
- フィルターの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認は開発者が管理したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** はプルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR のブロック要因に対応します。失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースを行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に内容を確認して、準備ができていると判断してください。

## Agent Merge で PR を管理する

コードがそろったら、まとめて確認し、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って agent merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功するとマージします。
7. "PR #14 was squash-merged successfully." のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発の複数の段階を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
