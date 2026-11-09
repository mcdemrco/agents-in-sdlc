---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織が技術的負債に悩んでいます。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に行います。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 明確な範囲の issue が重要な理由を確認します。
- Copilot に issue を割り当てます。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。最初の Web サイトを作成した委託先がドキュメントを残しておらず、アプリケーションにはほとんどドキュメントがありません。まず、アプリケーションでエクスポートされているすべての関数に TSDoc コメントを追加します。

デザイン チームは、ゲームを見つけやすくすることにも取り組んでいます。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、利用者がほかのゲームも引き続き見られるようにしたいと考えています。完成度の高い実装はまだ必要ありません。ユーザー体験 (UX) の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、優先度の高い issue がほかにあります。

どちらも優先順位が下がりやすいタスクで、Copilot cloud agent に適しています。Copilot cloud agent に非同期で作業させている間、開発者は別の作業に取り組み、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。[GitHub issue を Copilot に割り当てる][assign-issue]方法などがあります。割り当てると Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境を準備して作業を始めます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法のような力はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。人工知能 (AI) ペアプログラマーとして扱い、段階的に進めながら学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイダンスについて確認しました。カスタム指示を実際に試したことがあれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み取り専用で確認してください。後で結果をレビューするときに、その影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次の演習で `Code lacks documentation` issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、ファイル先頭のコメントが含まれていることを確認します。これらの指示ファイルについては、後の演習で PR をレビューするときにもう一度説明します。

## Copilot cloud agent の開発環境を準備する

コードの作成では、通常、必要なツールや依存関係を使えるように環境を準備します。Copilot にタスクを割り当てる場合も、同じようにセットアップが必要です。cloud agent もソフトウェア開発者と同様に作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に専用のセットアップ ワークフローを作成すると、作業開始前に必要なツールや依存関係を用意できます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザー、単一 Astro アプリのマイグレーション済み・シード済み SQLite データベースを Copilot が使えるよう、事前に設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなどでセットアップ手順がリポジトリをクローンする場合は、contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一 Astro アプリ用にローカル SQLite データベースをマイグレーションし、シードします
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。このような技術的負債は対応されないままになり、開発効率や保守性、新しい開発者の受け入れに影響します。Copilot はドキュメントの作成にも役立ちます。Copilot cloud agent に issue を割り当て、バックグラウンドでドキュメントを作成させましょう。後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   関数やメソッドでは、役立つ箇所に TSDoc コメントを記述することを組織の要件としています。残念ながら、最近の更新はこの標準に従っていません。動作を明確にできる箇所にコメントが含まれるよう、既存のコードを更新する必要があります。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエストを開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は先ほど説明したワークフローを実行して環境を作り、プロジェクトを分析して、最適な進め方を検討しています。

13. TODO リストを読み、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブ セッションをスクロールし、Copilot がどのように作業するか確認します。コードを調べて状態を理解し、方針を考え、コードを作成する様子が表示されます。

作業には数分かかる場合があります。cloud agent の主な利点は非同期で作業できることです。別の作業に取り組めるよう、この特長を利用して、もうひとつのタスクを割り当ててからアプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent に作業を分けて任せると、開発者は別のタスクに集中できます。デザイン チーム向けの関連ゲームの追加に時間がかからない場合でも、その時間を別の作業に使えます。cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

   ```markdown
   各ゲームの詳細ページに関連ゲームを表示し、ほかのゲームも見つけられるようにします。デザイン チームがユーザー体験を確認して、受け入れテストを行います。要件は次のとおりです。

   - 指定したゲームを除き、同じカテゴリのほかのゲームを返すデータアクセス ヘルパーを `src/lib/` に追加します。
   - ヘルパーを使う「Related games」セクションをゲーム詳細ページに表示します。
   - 関連ゲームがない場合に対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。cloud agent はソフトウェア開発者と同じように作業するため、完了まで常に監視する必要はありません。後で結果をレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使って、issue を非同期で Copilot に割り当てる方法を学びました。cloud agent は技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを支援します。

このレッスンでは、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 明確な範囲の issue が重要な理由を確認しました。
- Copilot に issue を割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
