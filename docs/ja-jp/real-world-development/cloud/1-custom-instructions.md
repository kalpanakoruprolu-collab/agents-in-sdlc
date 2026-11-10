---
title: "演習 1 - カスタム instructions (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[Instruction ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム instructions とパスを対象とする instructions ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、ここではプロンプトの「実行前と実行後」を比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、instructions ファイルの影響を確認します。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使用します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

Instruction ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行するために必要な情報を提供します。

### カスタム instructions

カスタム instructions を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に伝え、より適切な提案やコードスニペットを得られます。希望するコーディング規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する instructions や、特定の種類のファイルを対象にした instructions を作成できます。

Instruction ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の instruction ファイルです。技術スタック、開発内容の概要、ベストプラクティスなど、Copilot Chat や CLI へのほとんどの要求に関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスクに対するガイドラインを記載できます。

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

## 新しいリポジトリ標準を追加する

ここで行う編集は、プロジェクト全体に適用するルールを追加することです。コード内のドキュメントには TSDoc の doc comment とファイルレベルのコメントヘッダーを使うよう Copilot に指示します。まず、作業用のブランチを作成します (cloud agent は issue の対象ブランチから instructions を読み取りますが、変更をきちんとコミットします)。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## Instructions の更新をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチから instruction ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい instructions を参照できるよう、変更を `main` に反映する必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue を割り当てるとき、cloud agent はこの instructions を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず `main` にコミットしてもかまいません。実際のプロジェクトと同じ手順を体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りの演習で、cloud agent が作成するプルリクエストにその標準が反映されることを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム instructions を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム instructions を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
