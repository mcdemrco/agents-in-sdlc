---
title: "レッスン 8 - 機能のプルリクエスト (PR) を作成してマージする"
description: "フィルタリング、指示、スキルの更新、品質保証 (QA) プロファイル、テストをまとめてレビューし、プルリクエスト (PR) を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべてひとつのブランチに保存されています。まとめてレビューし、プルリクエスト (PR) を作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質改善の作業をひとつの会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、マージの承認権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている要因に対応します。継続的インテグレーション (CI) のチェック失敗を修正し、レビューコメントに対応し、必要に応じてリベースした後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せることもできます。有効にする前に、変更をレビューし、マージしてよい状態か判断してください。

## Agent Merge を使って PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべてのチェックに成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、不足している権限を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[GitHub Copilot CLI のスラッシュコマンド][next-lesson]を使い、コンテキスト、モデル、共有、任意のクラウドへの委任について確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
