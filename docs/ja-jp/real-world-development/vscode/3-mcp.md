---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。アプリを手動で操作する代わりに、**Playwright MCP サーバー**に接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

エディターの外部に接続できると、agent mode の機能が大きく広がります。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、大規模言語モデル (LLM)、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、最新情報へのアクセス (resources の利用) や、開発者に代わった操作 (tools の利用) が可能になります。

これらのツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用します。MCP サーバーは、AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカルツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる個別のツールやリソースを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するアプリケーションプログラミングインターフェイス (API) にアクセスできます。AI エージェントが新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作機能を提供します。AI エージェントが Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくするための [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティ面では、MCP サーバーをプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス権を付与するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれているため、前の演習で Copilot はバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに最適です。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` サーバーは**ローカル**で動作します。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは Playwright に画面を表示せずにブラウザーを実行するよう指示します。デスクトップ画面がない codespace では、このオプションが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright を使うため、Playwright が使用するブラウザーは通常すでにインストールされています。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うとき、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成をレビューし、信頼して起動します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを手動で操作して変更を確かめる代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を確認して報告できます。会話を離れずに機能が期待どおりに動作するか確認できるため、効率的です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素に基づいて判断します。支援技術と同じように動作するため、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーに接続し、アプリケーションを起動した状態で、次の手順で Copilot にフィルタリング機能を試してもらいます。

1. Copilot に次のプロンプトを送信します。

    ```text
    Playwright MCP サーバーを使って、実行中のアプリケーション http://localhost:4321 をブラウザーで開き、新しいゲームのフィルタリング機能を確認してください。

    1. ゲームページに移動し、一覧に表示されているゲームの数を確認します。
    2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが表示されることを確認します。
    3. フィルターを解除してからパブリッシャーフィルターを適用し、そのパブリッシャーのゲームだけが表示されることを確認します。
    4. カテゴリと publisher のフィルターを組み合わせ、両方が結果に反映されることを確認します。

    各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
    ```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らして報告を確認してください。問題があれば、プルリクエストを開く前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で実行しておく必要があります。

アプリの起動やブラウザーの準備が必要な場合は、次の手順で対応します。

1. 開発サーバーを停止している場合は、再起動してアプリを `http://localhost:4321` で実行します。
2. Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるようにプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot が行った変更を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を利用しているので、PR の作成を依頼しましょう。Copilot は関連 issue を探し、関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub のツールを使えるよう、エージェントドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように Copilot に PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関連する issue を探してください。現在のブランチから新しいプルリクエストを作成し、正しい issue に関連付けてください。
   ```

4. Copilot が情報を収集して操作を行うために必要な場合は **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。指示に従って許可します。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、まだマージ**しないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使い、出荷前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
