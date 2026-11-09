---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発フローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みのフィルタリング issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 要件、テスト範囲、スキルの結果、ブラウザーでの根拠を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルタリングの変更全体をレビューし、その PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## 提出した変更

ワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲームカードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` の報告をカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業をひとつの PR にまとめています。

## 異なる方法での検証

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー確認でコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果を、要件とテスト範囲のレビューと合わせて確認しました。

追加するテストは、実際に不足している範囲を補うものである必要があります。追加テストが不要という QA の結果が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前の検討、**Interactive** は変更に関わり続ける作業、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローをひととおり確認しました。Copilot CLI には、さらに次の機能があります。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

ツールの使い方を上達させるには、使い続けることが一番です。本番コード、個人のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと知見を共有し、ほかの開発者の経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
