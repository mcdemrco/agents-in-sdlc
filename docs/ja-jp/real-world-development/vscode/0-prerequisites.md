---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ワークショップ用リポジトリを設定する

コードを作成するためのリポジトリを用意します。[テンプレート][template-repository]から自分用のリポジトリを作成します。必要なファイルがすべて含まれるため、演習を進めながら使用できます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリ ページで **Use this template** ボタンを選択し、次に **Create a new repository** を選択して自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。演習中に参照します。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップではこの issue を使います。自分で issue を登録する必要はありません。

## codespace を作成する

演習では codespace を使います。

[GitHub Codespaces][codespaces] は、ブラウザーでコードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする統合開発環境 (IDE) が提供されます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で codespace に戻ります。ひとまず、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらにも演習に必要な前提条件がインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードを使うには、統合開発環境 (IDE) に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespaces を使っている場合は、通常すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、GitHub アカウントで認証する必要がある場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに `Hello world` のようなメッセージを入力し、<kbd>Enter</kbd> を押します。これで Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。表示される手順に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用のリポジトリを作成し、コードを書くときに使用する codespace の作成を開始しました。

## 次のステップ

次は Copilot に作業を依頼します。[演習 1 - カスタム指示][next-lesson]に進み、プロジェクトの規約を Copilot に伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
