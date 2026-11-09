---
title: "演習 3 - Playwright の Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。プルリクエスト (PR) を開く前に、ブラウザーで動作を確認します。手動でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストさせます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーで機能を手動テストします。
- ブランチを公開し、フィルタリングの PR を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるツールを使えると、さらに便利になります。Copilot が外部ツールを使う仕組みのひとつが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェントモードがコンテキスト、大規模言語モデル (LLM)、ツールと連携するしくみを示す図。MCP サーバーと VS Code 拡張機能が提供するツールを含む。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) エージェントは外部のツールやサービスと通信できます。外部のツールやサービスとリアルタイムでやり取りすることで、最新情報 (resources) にアクセスしたり、開発者に代わって操作を実行したりできます。

ツールやリソースには、MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスをつなぎます。たとえば、既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成や更新、issue やプルリクエストの管理などをエージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントに依頼できます。

ほかにも多くの MCP サーバーがあります。GitHub が提供する [MCP registry](https://github.com/mcp) で、エコシステム内のサーバーを探したり、貢献したりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されていることを確認します。

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

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]で Copilot が操作できるブラウザーを用意し、作成した機能をテストします。

1. `.vscode/mcp.json` で `github` と並べて `playwright` を追加し、ファイルが次のようになるようにします。

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

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、画面にブラウザー ウィンドウを表示せずに Playwright を実行できます。codespace には表示用のデスクトップ環境がないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーがすでにインストールされていることがほとんどです。Copilot がブラウザー不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。初めて Copilot がサーバーのツールを必要としたときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` の上に **Start** が表示されたら選択し、サーバーを起動して設定を確認します。
2. VS Code でサーバーを信頼するか確認されたら、設定を確認して信頼を選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを使う際に VS Code がサーバーを起動し、信頼の確認を求めます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを手動で操作しなくても、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認した内容を要約できます。会話を続けながら機能を確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが判断します。支援技術と同じ情報を使うため、機能の確認は簡単なアクセシビリティ確認にもなります。

MCP サーバーが接続され、アプリが実行中であることを確認して、Copilot に次の手順でフィルタリング機能を試させます。

```text
Playwright MCP サーバーを使って、実行中の http://localhost:4321 にブラウザーでアクセスし、新しいゲームのフィルタリング機能を確認してください。

1. ゲームのページに移動し、一覧にいくつのゲームが表示されているか記録します。
2. カテゴリで絞り込み、そのカテゴリのゲームだけが表示されることを確認します。
3. フィルターを解除し、パブリッシャーで絞り込んで、そのパブリッシャーのゲームだけが表示されることを確認します。
4. カテゴリとパブリッシャーの両方を指定し、両方の条件が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を確認して結果を報告します。issue の受け入れ条件に照らして報告を読みます。問題がある場合は、プルリクエストを作成する前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が初めて Playwright MCP サーバーを使うとき、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるよう PR を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更されたファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストの作成方法には、github.com や GitHub コマンド ライン インターフェイス (CLI) を使う方法などがあります。ここでは GitHub Copilot に作成を依頼します。関連する issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub ツールを使えるように、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトを送り、PR を作成するよう Copilot に依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関する issue を探してください。現在のブランチから PR を作成し、該当する issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作を Copilot に許可します。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。画面の手順に従ってください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- MCP サーバーを使い、ブラウザーでフィルタリング機能をテストしました。
- ブランチを公開して、フィルタリングの PR を作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法のひとつです。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティのレビューなどに特化した作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
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
