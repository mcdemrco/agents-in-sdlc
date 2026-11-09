---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

これまでの演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つのタスクを割り当てました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェント ページ][agents-page]では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を送ることもできます。

この演習では、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラスト モードの作成をエージェントに依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。ハイコントラストに加えてライトモードも実装するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン パネルと左側のパネルにあるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲームを変更する API の生成
   - Web サイトへのハイコントラスト モードの追加
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次の操作などが含まれます。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - Model Context Protocol (MCP) サーバーの設定
   - 割り当てられたタスクを完了するための作業

> [!NOTE]
> 表示される手順は Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) パネルも確認します。PR と変更されたファイルを監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認したので、ハイコントラスト モードの作業中にライトモードのトグルも追加するよう Copilot に依頼します。

1. ハイコントラスト モードを追加するセッションを選択します。エージェントが付けた名前や作業状況によって、タイトルは異なります。

   ![ミッション コントロールに表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   ハイコントラスト モードに加えて、ライトモードも追加してください。ユーザーが希望する表示モードを選べるように、ライトモード用の切り替え機能も用意してください。
   ```

   ![Copilot の作業中にセッションを調整するダイアログが強調表示された、agents ページのタスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot に作業を続けさせる

更新したタスクに取り組むよう、Copilot が作業を続けます。メッセージを送信した時点でエージェントが実行中の手順を終えると、新しい依頼が作業に追加されます。

前の手順と同様に完了まで数分かかります。その間、これまでに学んだことを振り返ることもできます。

## まとめと次のステップ

Copilot agents ページを使い、GitHub Copilot cloud agent のタスクを監視して方向付ける方法を学びました。進行中と完了済みのタスクを追跡し、作業を確認して、実行中のタスクを調整できます。

この演習では、次の操作を行いました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
