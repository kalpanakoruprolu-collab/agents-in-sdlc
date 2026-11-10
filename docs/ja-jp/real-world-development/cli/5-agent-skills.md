---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果の書式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードの検証が必要です。これまではコードの動作を手動で確認し、命令ファイルを使って標準に従うようにしました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには**エージェント スキル**が適しています。スキルは、こうした操作を正しく実行する方法を Copilot に伝えます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys はユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む再利用可能なタスク命令、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキルの名前と説明を定義する frontmatter、機能の概要、呼び出すタイミングのガイダンスが含まれます。フォルダーにスクリプトやほかのリソースを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクト既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルはプロジェクトの `.github/skills` に配置し、チームで共有・再利用するリポジトリ資産にできます。または、ユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、テストと lint の手順を Copilot にどう案内するか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。Copilot に 3 種類のチェックを実行させます。

1. フィルターの会話に戻り、Interactive モードにします。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したか、ツールが実際にどのように報告したかが分かるレポートにします。スキルを更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を実際に試します。Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。このレッスンでは、次の操作を行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルター機能と一緒に機能 PR に含めます。次は、Copilot がサイトを直接操作できるよう[Playwright MCP サーバー][next-lesson]を利用します。

## その他のスキル例

コミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と一括エクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
