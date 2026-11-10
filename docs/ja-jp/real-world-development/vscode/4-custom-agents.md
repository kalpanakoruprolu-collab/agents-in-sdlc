---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を徹底するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、概念上の共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。最もわかりやすい区別は、**カスタムエージェントは作業者、スキルはツール**ということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一環としてスキル (ほかのエージェントを含む) を統括できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフローのスキルや、テストを実行して管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの活用方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近のフィードバックによると、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくい利用者がいます。このアクセシビリティの課題に対応するため、デザインチームは利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にはカスタムエージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。Copilot を導く方法を理解するため、内容を確認しましょう。

アクセシビリティ作業をフィルタリングのプルリクエスト (PR) と分けるため、codespace で `main` から新しいブランチを作成します。

1. codespace に戻り、ターミナルを開いて、次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

4. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコード生成に関する主な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタムエージェントが Copilot Chat ビュー下部のエージェントドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントの instructions とツールを使うチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェントドロップダウンで、**Agent** が選択されていることを確認します。選択されていない場合は選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェントドロップダウンを選択します (現在のエージェントが表示されます。既定では **default** です)。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    サイトにハイコントラストモードを追加してください。利用者が切り替えられるハイコントラスト用のトグルを設け、設定をブラウザーのローカルストレージに保存してページの再読み込み後も保持してください。
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更ごとに、ファイルパスと差分がチャットに表示されます。
6. 変更が適用されると **Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリのファイルを実際に変更します。作業中に Astro コンポーネント、CSS、関連テストなどが更新されます。

次の演習では、進行中の作業をレビューし、追加の指示を出します。

## まとめと次のステップ

このレッスンでは GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に合わせた AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、特定の種類の作業をより効果的に行うよう Copilot を導けます。

次の概念を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して方向づけ][next-lesson]ます。変更を確認しながら、同じセッションにライトモードのトグルを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
