---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、登録済みのフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行して、外部ツールに接続できます。すべてコマンドラインから行えるため、タスクを任せたり、変更を依頼したりしながら集中して作業できます。最初の手順としてツールをインストールします。すでに使い慣れているツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub Model Context Protocol (MCP) サーバーでフィルタリングの issue を見つけます。

## シナリオ

チームは、増え続けるバックログに対応するため人工知能 (AI) エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでエージェントの機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイス コードを使った認証手順に従い、Copilot CLI を認証して許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問を Copilot に送り、正常に動作することを確認します。

   ```plaintext
   このプロジェクトの重要なファイルは何ですか。
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトでフィルタリングの issue を探します。

   ```plaintext
   GitHub MCP を使って、このリポジトリで "Allow users to filter games by category and publisher" というタイトルの issue を探してください。URL と簡単な要約を教えてください。何も変更しないでください。
   ```

7. URL を開いて issue を読みます。最初の変更を行った後で使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルでチェックアウトされているブランチを使います。自動的に worktree が作成されるわけではありません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既定の許可を超えるツールの使用前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。Codespace 内で承認プロンプトを省略し、演習に集中するためのショートカットです。

> [!CAUTION]
> `--yolo` はすべての権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を自動的に有効にします。Codespace や仮想マシン (VM) のような分離された環境でのみ使い、日常の開発で既定のエイリアスにしないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

後の演習では issue とプルリクエストを扱うため、`--enable-all-github-mcp-tools` を指定して、GitHub MCP の読み取り・書き込みツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. もう一度プロジェクトについて簡単な質問を送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後は、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の操作を行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルタリングの issue を見つけました。

次は[最初の変更を行い][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
