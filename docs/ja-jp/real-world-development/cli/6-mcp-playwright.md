---
title: "レッスン 6 - Playwright の Model Context Protocol (MCP) で機能を検証する"
description: "Copilot CLI で Playwright の Model Context Protocol (MCP) サーバーを確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要があります。ここで役立つのが Model Context Protocol (MCP) サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能だけでなく、ほかのツールやサービスも利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、ユーザー インターフェイス (UI) の変更を検証するには、実際に UI を操作する必要があります。Copilot が開発中の Web サイトをユーザーと同じように使えるようにし、期待どおりの変更をより確実に作成できるようにします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは外部サービスとリアルタイムにやり取りし、最新情報にアクセスしたり、開発者に代わって操作を実行したりできます。

エージェントは MCP サーバーを介してツールやリソースにアクセスします。MCP サーバーはエージェントと外部ツールやサービスをつなぎ、それぞれが利用可能なツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub が提供する [MCP registry][mcp-registry] で、エコシステム内のサーバーを探したり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう、サーバーを追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

これまでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot CLI で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを送り、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らし、フィルタリング機能を確認してください。変更を加えずに、動作する点としない点を報告してください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は Copilot が MCP サーバーを自動的に判断するため、明示的に指定する必要はありません。使用するサーバーが分かっている場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を表示する様子を見ます。
3. 報告を読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使い、Copilot CLI から実際のブラウザーで機能を確認しました。このレッスンでは、次の操作を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザーツールを専門的な役割で組み合わせる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
