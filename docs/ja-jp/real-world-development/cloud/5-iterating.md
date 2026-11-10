---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善や機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を実装し、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計や DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って進めます。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つためのセキュリティ制約が設けられています。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限定されます。
- Cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードにドキュメントを追加した変更を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定された動作です。Copilot は、外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されています。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。Copilot の作業が完了するまで待ってください。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりできます。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、新たに追加されたドキュメントを確認します。実際の変更は環境によって異なります。

    変更を確認しながら、TSDoc の doc comment、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム instruction ファイルに基づいています。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストで協力する方法は、レビューだけではありません。プルリクエストへのコメントやコードへのインラインコメントで、チームのほかのメンバーと同様に Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、必ず成功する指示文は提示できません。たとえば、次のような変更を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにメンションし、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例を使うか、コードベースに必要な別のドキュメント改善を依頼します。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. プルリクエストに戻るには **Back to pull request** を選択します。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了したら、プルリクエストに新しいコミットが追加されたことを確認します。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで改善を繰り返してください。問題がなければ、PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージできます。

![レビュー可能な状態に PR を変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲームの詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 一部のワークフローが承認待ちになっていることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** 関連ゲーム機能を手動でテストするために、Codespace でこのブランチに切り替えることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の更新を依頼して、作業を繰り返し改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc の doc comment を追加したりできます (この issue はカスタム instructions を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の mission control で追加を依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** の表示があります。Copilot の作業が完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存するローカルストレージの使用方法
   - ハイコントラストモードとライトモードの Cascading Style Sheets (CSS) またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** アクセシビリティ機能を手動でテストするために、Codespace でこのブランチに切り替えることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替え、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティに関する問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

Cloud agent を使うと、バックログの実際の作業を任せて、別のタスクに集中できます。さらに経験を積むため、リポジトリに issue をいくつか登録して Copilot に割り当ててみてください。たとえば、次のタスクを試せます。

1. ゲームの詳細ページに支援者向けの関心フォームを作成します。
2. ゲーム一覧ページにページネーションを実装します。
3. データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- このリポジトリに含まれるカスタム instruction ファイルを確認し、後で cloud agent の出力に及ぼす影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使用し、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティセッションを監視して作業中に方向づけました。
- cloud agent のプルリクエストをレビューして改善し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent と連携する任意の `/delegate` ブリッジを使って、レビュー済みの 2 つの変更をターミナルから実施します。

自分のリポジトリでは、次のフォローアップを試してください。

1. リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
2. 別の分野向けのカスタムエージェントを作成します。
3. 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
