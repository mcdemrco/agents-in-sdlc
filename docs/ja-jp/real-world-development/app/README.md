---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションです。エージェントを使った開発を、集中して作業できる単一のワークスペースで行えます。エージェントの並列セッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブな管理機能を利用できます。**Agent Merge** は、リベース、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージまで、プルリクエストの処理を支援します。

このワークショップでは、Tailspin Toys の一連の開発フローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリに接続し、ワークスペースと登録済みのバックログを確認します。
2. 星評価を小さく変更し、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright の Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリング機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、テスト範囲、検証の根拠をレビューします。
8. フィルタリングの変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバスを利用できるか確認した後、Database Explorer を使い、リポジトリと連携するトリアージ用キャンバスを作成してテストします。必要な機能を利用できない場合は、利用できない条件を記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップでは、星評価とフィルタリングの 2 つの PR を作成します。2 つ目には、指示、スキル、QA プロファイル、テストの更新も含めます。各 PR は更新済みの `main` から開始します。フィルタリングと品質改善の作業では、同じセッション、worktree、ブランチを使い、各ツールを学びながら作業を続けます。最後のキャンバス演習も同じセッションで行い、PR の手順を繰り返すのではなく、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトに接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、1 つ目の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、報告形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | **Customize** から MCP を設定し、フィルタリングの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門的なプロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリと連携するトリアージ用キャンバスを作成してテストします |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加リソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は codespace ではなく自分のコンピューターで動作します。そのため、[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者がアプリを利用できるように **GitHub Copilot app** ポリシーを有効にする必要があります。このポリシーは Copilot CLI のポリシーとは別です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
