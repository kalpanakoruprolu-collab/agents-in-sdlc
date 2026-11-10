---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更を行い、テストを実行し、デバッグし、変更し忘れたファイルを見つけて、さらに更新するという作業を繰り返します。

Copilot Agent Mode は、このようなときに役立ちます。

Copilot Agent Mode は統合開発環境 (IDE) 内でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに設計されています。Agent mode は IDE 内で作業するため、すばやいフィードバックを得て対話できます。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じて開発者とやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行います。
- GitHub Copilot Agent Mode でユーザーインターフェイス (UI) とデータ層にまたがる新機能を実装します。
- 生成された変更とテストをレビューしてから、コードベースにマージします。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。ここでは、Copilot Agent Mode を使って残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。このサイトは単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language (HTML) としてレンダリングします。ページはフロントマターから Drizzle オブジェクトリレーショナルマッピング (ORM) を通じてローカルの SQLite データベースを直接クエリします。別のバックエンドのアプリケーションプログラミングインターフェイス (API) やクライアント側の UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の Document Object Model (DOM) API を使った、スコープが限定された小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習を終えた `main` ブランチの codespace に戻ります。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択して、Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、publisher、カテゴリなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数のファイルにわたる変更の計画と実装、ターミナルコマンドの実行、ツールの呼び出しを自律的に行います。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が、特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、体験を揃えやすくする目的で手順に含めています。ただし、大規模言語モデル (LLM) の出力は確率的であり、ワークショップに記載したものとは異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の出力は確率的なため、Copilot が別の MCP コマンドを使用する場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   GitHub リポジトリのバックログにある項目を表示してください。利用者にとって最も役立つものを基準に、優先順位を付けるのを手伝ってください。
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有益だと考える項目を基準に、優先順位まで付けたことを確認します。

## Instruction ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを伝える instruction ファイルを確認します。Web サイトに機能を追加する方法に関する情報を含む[ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のアプローチに関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - Astro、Tailwind CSS、Drizzle などのファイル向け instruction ファイルへのリンク。

> [!TIP]
> Instruction ファイルから、ほかの instruction ファイルやプロジェクト内のファイルを参照できます。パスは instruction ファイルからの相対パスです。この方法で再利用を促し、複雑な instructions を管理しやすい単位に分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくともアプリケーションの次の 3 か所を更新します。

- データ層 (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリング動作のテストを追加または更新します。
- ゲーム一覧ページを更新し、フィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instructions ファイルを指定します。

   ![UI instructions ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルも **Claude Sonnet 4.5** のまま選択されていることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   関連する GitHub issue の要件に基づき、パブリッシャーとカテゴリによるフィルタリング機能をサイトに追加してください。前の演習で追加した publishers ヘルパーを維持し、必要に応じて改善しながら、残りのデータ層、UI、テストを完成させてください。完了前にすべてのテストが成功することを確認してください。サーバーはすでに起動しているため、起動し直す必要はありません。
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを特定する様子を確認します。データ層のヘルパー、UI、テストを探してから、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行など、複数のタスクを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** ボタンが表示される場合があります。Copilot の作業中に選択する必要はありません。作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテストの実行を確認するダイアログが表示された画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業の途中で一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装します。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行います。
- 生成された変更とテストをレビューしてから、コードベースにマージします。

次は [Playwright Model Context Protocol (MCP) サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだ内容を使い、Copilot にサイトのページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 既存の instruction ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズを利用者が選べるようにするか、固定にするかなど、ページネーションの方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼してコンテキストを提供しながら、繰り返し改善する必要がある場合があります。これは Copilot を利用する際の通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
