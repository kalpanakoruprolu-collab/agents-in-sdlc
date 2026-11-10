---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加してフィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で進める必要があるなら、その指針を Copilot に与えます。[命令ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。フィルター機能を実装したので、Copilot が使った命令を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパス指定の命令がエージェントに適用される方法を確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスのガイドラインがあります。

- コメントはコードの言い換えではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使い、Copilot がこれらのプラクティスに従うための情報を提供します。

## 命令ファイル

カスタム命令は、Copilot がコーディング スタイルと要件を理解し、適切な提案を行うためのコンテキストと設定を提供します。好みの規約、ライブラリ、コメントの書き方を指定できます。リポジトリ全体、または特定のファイルやタスク向けに命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべての**要求で Copilot に送信される単一ファイルです。多くの要求に関係するプロジェクト情報を含めます。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル種別、タスクに適用するガイドラインです。

> [!NOTE]
> ほかの命令形式とそのサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが用意されています。変更前に内容を確認し、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。ここで命令を適用するファイルの glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな命令セットを再利用しやすい小さな単位に分け、例を示せます。

## チームのガイダンスに合わせて命令を更新する

既存のファイルは出発点として十分ですが、TypeScript の新規コードに関する指針がありません。`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を見つけます。
2. 最後の箇条書きとして次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。更新した命令を保存したら、フィルター機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルターの会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの命令とパス指定の命令がエージェントに適用される方法を確認しました。
- コーディング標準が守られるように命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、lint とテストが一貫して実行されるよう、再利用可能な `quality-checks` スキルを[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [効果的なカスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot - 命令やリソースのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
