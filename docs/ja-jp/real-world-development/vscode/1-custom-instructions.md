---
title: "演習 1 - カスタム instructions (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが、コード生成に関するガイダンスを提供する[Instruction ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム instructions とパスを対象とする instructions ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の instructions を使って、フィルタリングに必要な最初のデータスライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードが新しい標準に従うことを確認します。
- Instructions の更新とフィルタリングの基礎となるコードを `main` にコミットし、Copilot が次の演習で利用できるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot の出力は確定的ではありません。この演習では、更新前後で出力の一字一句が一致することではなく、instructions の更新後に動作がどのように変化するかを確認します。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使用します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

Instruction ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行するために必要な情報を提供します。

### カスタム instructions

カスタム instructions を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に伝え、より適切な提案やコードスニペットを得られます。希望するコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する instructions や、特定の種類のファイルを対象にした instructions を作成できます。

Instruction ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の instruction ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、Copilot Chat や CLI へのほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、ユーザーインターフェイス (UI) コンポーネントの作成やユニットテストの追加などのタスクに対するガイドラインを記載できます。

> [!NOTE]
> 統合開発環境 (IDE) では、instructions ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの instructions と `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の instructions ファイル][custom-instructions-support]にも対応しています。

### Instructions ファイルを管理するためのベストプラクティス

Instructions ファイルの作成方法をすべて解説することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発内容、プロジェクト構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスクに関する instructions を記載します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

Instructions ファイルや AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な数の instruction ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルが見つかります。
>
> Copilot に instruction ファイルを生成させることもできます。操作方法は利用する環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。各環境のレッスンで、関連する操作を説明します。
>
> テンプレートやひな形を探している場合は、instruction ファイル、カスタムエージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instructions ファイルを確認する

このリポジトリに含まれる instruction ファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスクを対象にした `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらは、Copilot とやり取りする際に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する instructions があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対パスで対象ファイルを指定する glob です。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成する際の instructions を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の instruction ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように大きな instruction セットを小さく再利用可能なファイルに分け、コード生成時の参考となる例にリンクできます (パスはリポジトリのルートではなく、instruction ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

変更にあたって、まずブランチを作成するというベストプラクティスに従います。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## Instructions の更新前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム instructions の効果を確認するため、まず現在の instruction ファイルを使ってプロンプトを送信します。その後、ファイルを更新して同じプロンプトを再送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを取得できるよう、前の演習で開いたエディターのタブを閉じます。
2. ヘルパーを配置する場所を Copilot が把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべての publisher の名前と id を返すデータアクセスヘルパーを追加してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストなど、複数のファイルに変更が及ぶ場合もあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的であり、指示がなくても doc comment が追加される場合があります。その場合も問題ありません。instructions 更新後の一貫性の向上を確認することが目的です。

## 新しいリポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コーディング標準を明記して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。これが、生成されたヘルパーに doc comment がなかった理由です。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリレベルの `.github/copilot-instructions.md` や、言語、ファイルの種類、タスクを対象とする `*.instructions.md` ファイルを作成できます。今回追加した doc comment のルールなど、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をクリックして、Copilot が適切なファイルに注目できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべての publisher の名前と id を返すデータアクセスヘルパーを追加してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で発展させる実際のコードに適用しました。

## Instructions をコミットしてブランチをプッシュする

Instruction ファイルもリポジトリのほかのアセットと同様に、ソース管理で管理します。Instructions とヘルパーの変更をリポジトリにコミットして、ブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、instructions とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、instructions とヘルパーの基礎となる変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで次のコマンドを実行し、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けたプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を見つけ、issue をクローズする PR を作成するようプロンプトで依頼します。その後、PR をマージします。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートには GitHub のツールがあらかじめワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従って許可してください。後の演習では、この接続の仕組みである Model Context Protocol について説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. instructions ファイルの更新に関する issue を探し、現在のブランチからプルリクエスト (PR) を作成するよう Copilot に依頼します。PR には instructions の更新と、新しい publishers ヘルパーの基礎を含めます。

   ```
   instructions ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その issue をクローズすることを明記してください。また、今後のフィルタリング作業に備えて publishers ヘルパーの基礎も追加したことを記載してください。
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot がユーザーに代わって GitHub 操作を実行する許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、PR を `main` にマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新の状態にしてください。
   ```

7. Copilot が GitHub 操作やシェルコマンドの実行許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- コード生成プロンプトを送信し、**既存の** instructions から Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、instructions の更新とヘルパーの基礎を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加する際に、[agent mode][next-lesson]で instructions を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム instructions を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム instructions][personal-instructions]
- [Awesome Copilot — instruction ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
