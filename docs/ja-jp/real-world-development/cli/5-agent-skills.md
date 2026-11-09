---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードを手動で検証したり、指示ファイルでコーディング標準に従わせたりする作業があります。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、これらの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使います。チームは結果を読みやすくするため、報告を改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースがまとめられています。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキルの内容と用途を示す `name` と `description` の frontmatter、機能の概要、呼び出すタイミングのガイダンスを記述します。スキル フォルダーには、使用するスクリプトやその他のリソースを置くサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは既存のプロジェクト コマンドを使うため、`SKILL.md` だけで構成されています。

スキルはプロジェクトの `.github/skills` に置くと、チームで共有して再利用できます。ユーザーのスキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。`description` は、Copilot がスキルを呼び出すタイミングを判断するために使います。
3. 指示を読み、テストと lint の手順を Copilot にどう案内するか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更する前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。まず、3 つのチェックを実行するよう依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   ユニットテスト、lint、型チェックについて、quality-checks スキルを実行してください。
   ```

3. 最後に表示される報告を確認します。

## 報告をカスタマイズする

実行したチェック、成功したかどうか、ツールの実際の報告内容が分かる形式に改善します。スキルを更新して、その形式で報告するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを実際に使って確認します。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前と**まったく同じプロンプト**を送信します。

   ```plaintext
   ユニットテスト、lint、型チェックについて、quality-checks スキルを実行してください。
   ```

3. 最後に表示された報告を読み、最初の報告と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次の操作を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリング機能とともに機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作する方法を確認します。

## その他のスキルの例

以下のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と一括エクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
