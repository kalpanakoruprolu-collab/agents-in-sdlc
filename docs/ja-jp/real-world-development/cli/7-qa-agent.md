---
title: "レッスン 7 - 品質保証 (QA) エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、明確な品質保証 (QA) プロセスを持つカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター issue に照らして結果を確認します。

## シナリオ

Tailspin Toys では、機能 PR を開く前に要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使えば QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルにはエージェントの目的、命令、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチーム標準を説明します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告する方法を Copilot に指示します。ほかの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、Copilot に再利用可能な QA プロファイルを作成させます。プロファイルには QA が行うチェックと守るべき境界を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界をプロファイルに反映したことを確認します。こうすることで、検証だけを行いたい機能を不完全または範囲の広すぎるエージェントが変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、設定すると分かりやすい表示名になります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示し、実装コードの変更前に確認を求め、変更をコミットせず PR も開かないことを確認します。
5. これらの役割や境界が欠けている場合は、続行する前に既定のエージェントへ修正を依頼します。

## issue を基に QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。フィルターの会話を再開して QA を選択すると、コンテキストに含まれる issue と計画の決定を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したらレポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その作業を確認しました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター issue に照らして結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがレビュー可能な状態になりました。次は、これらを機能 PR にまとめ、[Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
