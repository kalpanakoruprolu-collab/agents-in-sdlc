---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。[codespace][codespaces] で Tailspin Toys リポジトリのコピーを作成し、instructions ファイルを編集したり、cloud agent の作業をレビューしたりできるようにします。

## ラボ用リポジトリをセットアップする

作成するコードのためにリポジトリのコピーを用意します。[テンプレート][template-repository]から新しいリポジトリを作成すると、ラボに必要なファイルがすべて含まれます。演習中はこのリポジトリを使用します。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボリポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加する場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモしておきます。ラボの後半で参照します。

## codespace を作成する

続いて、ラボの演習を進めるために codespace を使用します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを記述、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した高機能な統合開発環境 (IDE) が用意されています。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに短時間で済みます。作成を待つ間、次に取り上げる GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 次の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] 内で実行できるように構成されています。どちらの環境にも、円滑に進めるために必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。VS Code が codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボリポジトリのコピーを作成し、Copilot cloud agent と一緒に作業するための codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム instructions を追加します。[演習 1 - カスタム instructions][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
