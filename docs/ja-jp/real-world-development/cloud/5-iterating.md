---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot にユーザー体験の改善と機能追加を依頼しました。コードへのドキュメント追加、デザイン チームが確認できる関連ゲーム機能の作成、ハイコントラストとライトモードを含むアクセシビリティ機能の実装を依頼しています。変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成系人工知能 (AI) を利用しても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者がいない状態で作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスは作業用のブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な独立環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- 既定では、[外部リソースへのアクセスは制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。Model Context Protocol (MCP) サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は既定で外部リソースや外部 MCP サーバーへのアクセスが制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントやその他のドキュメントを確認します。変更内容は状況によって異なります。演習 2 の最初に確認したカスタム指示ファイルによる TSDoc コメント、TypeScript の規約、ファイル先頭のコメントが含まれているか確認します。
6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は、変更を受け取るだけではありません。チーム メンバーに依頼する場合と同様に、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを受け取り、変更に対応する新しいセッションを開始します。結果は確率的に変わるため、指定の文面を推奨することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を短く説明するコメントを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例、またはコードベースに追加したいドキュメントについての別の依頼を使い、**@copilot** をメンションして生成されたドキュメントの変更をコメントします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

   ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

満足できるまで、Copilot とのやり取りを続けてください。完了したら、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージできます。

![レビュー可能な状態に変更するボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが追加されることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同じように、コメントを追加して更新を依頼し、cloud agent と反復できます。たとえば、表示する関連ゲームの数の変更や、独自の指示を追加する前に割り当てた issue への TSDoc コメントとファイル先頭のコメントの追加を依頼できます。**Conversation** タブでコメントすると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 でミッション コントロールから追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - 各モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラスト モードとライトモードの CSS またはスタイルの変更
   - Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript または TypeScript コード

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されることを確認します。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動して、ブラウザーでハイコントラストとライトモードのトグルを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベストプラクティスに沿った機能実装をどのように支援したか確認します。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 - さらに作業を委任する

cloud agent は、バックログの実際の項目を任せている間に別の作業に取り組めると便利です。いくつか issue を作成して Copilot に割り当て、実際に使ってみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の環境で、次のレッスンを完了しました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を確認しました。
- GitHub Copilot cloud agent に issue を割り当て、環境の準備、計画、非同期の作業を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードのトグルを追加しました。
- Copilot agents ページをミッション コントロールとして使い、アクセシビリティのセッションを監視して作業中に方向付けました。
- cloud agent のプルリクエストをレビューして反復し、**@copilot** をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent の環境での作業は完了です。引き続き確認する場合は、ほかの環境のワークショップも試してください。

- 🖥️ **[VS Code の環境][vscode-harness]** — IDE で Copilot Chat のエージェントモードと MCP の連携を確認します。
- 💻 **[CLI の環境][cli-harness]** — Plan と Autopilot、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge を使い、ターミナルからレビュー済みの変更を 2 つ届けます。任意で `/delegate` を使い、cloud agent に委任します。

自分のリポジトリでは、次の方法も試してください。

- リファクタリングやテスト範囲の拡充に関する issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベストプラクティス][cloud-agent-best-practices]
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
[cli-harness]: ../../cli/
[vscode-harness]: ../../vscode/
