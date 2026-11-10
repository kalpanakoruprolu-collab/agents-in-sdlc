---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼して、方向づけます。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているところなので、ライトモードも含めるのが自然です。エージェントの作業に追加の指示を出し、ハイコントラストに加えてライトモードも追加してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。Copilot に割り当てた次のタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを変更するためのアプリケーションプログラミングインターフェイス (API) を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行したタスクを確認します。次のような操作が含まれます。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - Model Context Protocol (MCP) サーバーをセットアップする。
    - 割り当てられたタスクを完了するための各種作業を行う。

> [!NOTE]
> 表示される具体的な手順は、Copilot の作業状態や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここでは、PR と変更されたファイルを表示し、追加の監視ができます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。Copilot が付けた名前や作業状態によって、タイトルは異なります。

    ![Mission control に表示されるアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードに加えて、ライトモードも追加してください。このモードにも切り替え機能を設け、利用者が希望する表示モードを選べるようにしてください。
    ```

    ![Copilot が作業中のエージェントタスクと、指示用のダイアログが表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込む様子を確認します。

## Copilot の作業を待つ

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順を終えると、新しい依頼が作業に反映されます。

今回も数分かかります。作業を待つ間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して方向づけるための中心的な場所である Copilot agents ページを確認しました。Mission control で、進行中および完了したタスクを追跡し、実行中の作業をレビューし、進行中のタスクに追加の指示を出して範囲を調整できます。

次の概念を確認しました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
