---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instructions][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装の時間が取れずにバックログに残った機能要求などが挙げられます。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるようにドラフトのプルリクエスト (PR) を作成します。面倒な作業を任せて開発を加速し、開発者がより大きな課題に集中できるようになります。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 明確に範囲を定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。サイトの初期版を作成した委託開発者は、ドキュメントをほとんど残していませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc の doc comment を追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトルを「関連ゲーム」として表示し、利用者がゲームを見続けられるようにします。現時点では、受け入れテストでユーザーエクスペリエンス (UX) を確認できる程度の実装が必要です。この作業は現在のブロッカーですが、優先度の高い issue がほかにあります。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、非同期に作業を進めてもらい、開発者はほかのタスクに集中できます。その後、戻って作業をレビューし、期待どおりの結果か確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフト PR を作成し、環境をセットアップしてタスクに取りかかります。実行中でも完了後でも Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、Copilot が PR で開発者に通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を適切に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム instructions

これまでの演習では、カスタム instructions と、Copilot を導く方法を確認しました。カスタム instructions の演習をすでに行っていれば、`.github/copilot-instructions.md` が機能することを確認できています。まだの場合は、この機会に簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction ファイルを読み取り専用で確認して、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI または起動済みの codespace で、次の instruction ファイルを読み取り専用で確認します。

1. `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` フロントマターで instructions の適用対象が `**/*.test.ts` に限定されていることを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てる際、作成された PR に TSDoc の doc comment、TypeScript の規約、コメントヘッダーが反映されているか確認してください。これらの内容は instruction ファイルから読み込まれます。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は特定の開発環境や、環境が正しく動作するために実行するセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使用します。作業を開始する前に実行する[専用のセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に設定して、この環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボの手順を円滑に進め、学習に役立てるため、あらかじめ設定されています。単一の Astro アプリで Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとデータ投入済みの SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot は独自のトークンを操作に使用します。セットアップ手順に必要な最小限の権限を設定してください。
    permissions:
      # たとえば依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一の Astro アプリのビルドとテストで使うローカル SQLite データベースをマイグレーションし、データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同じように見えますが、いくつかの重要な点があります。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot が PR で作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。このトリガーを使うと、Copilot の実行を待たずに、リポジトリの **Actions** タブからワークフローを手動で実行してテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、ドキュメント自体がなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    関数やメソッドには、必要に応じて TSDoc の doc comment を含めるという組織の要件があります。しかし、最近の更新ではこの標準が守られていません。動作を明確にするために必要な箇所に doc comment を追加してください。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーウィンドウを更新してください。
12. 数分後に、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行します)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新しないと、状態の更新が表示されない場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調べて状態を把握し、計画を考えて判断を下し、コードを作成している様子を確認できます。

作業には数分かかります。Copilot cloud agent の主な目的の 1 つは、非同期に作業して開発者が別のタスクに集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は、それほど時間がかからないかもしれませんが、その時間を別のタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲームを見つけやすくするため、各ゲームの詳細ページに関連ゲームを表示します。デザインチームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` に、指定されたゲームと同じカテゴリに属し、そのゲーム自体を除外したほかのゲームを返すデータアクセスヘルパーを追加する
   - そのヘルパーを使って、ゲームの詳細ページに「Related games」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーのユニットテストを作成する
   - PR を作成する前に、すべてのテストが成功することを確認する
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![issue に Copilot が取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。続いて、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。Cloud agent に issue を割り当てて、非同期に処理させることができます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などに活用できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 明確に範囲を定めた issue の重要性。
- issue を Copilot に割り当てる方法。

Cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム instructions も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
