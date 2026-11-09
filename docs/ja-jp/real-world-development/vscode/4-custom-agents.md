---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、エージェントに焦点を絞った指示、ベストプラクティス、コーディング パターン、分野固有の知識を与えます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] の準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、テスト パターンの一貫性を保つテスト エージェントなど、チームの専門知識を再利用できるエージェントに反映できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー全体に適用する `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを編成して作業できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローを扱うスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるよう取り組んでいます。現在のダークテーマは、テキストと背景色のコントラストが不十分で、読みにくいという意見がユーザーから寄せられました。デザイン チームは、ユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、この機能をすばやく実装します。カスタムエージェントに機能の生成を依頼します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタムエージェントをレビューします。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認し、Copilot を導く指示を理解します。

この演習ではフィルタリングの PR と分けて作業するため、`main` からアクセシビリティ用のブランチを作成します。

1. Codespace に戻り、ターミナルを開きます。
2. 次のコマンドを実行して `main` を更新し、アクセシビリティ用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. `.github/agents/accessibility.md` を開きます。
4. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

5. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したカスタムエージェントが、Copilot Chat ビュー下部のエージェント選択メニューに表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールを使うチャット セッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

1. Chat ビューのエージェント選択メニューで、まだ選ばれていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   サイトにハイコントラスト モードを追加してください。ユーザーが切り替えられるトグルを用意し、ブラウザーの local storage を使って、ページを再読み込みしても設定を保持してください。
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトのファイルに適用します。変更はファイル パスと差分とともにチャットに表示されます。
6. 編集が適用されると、エージェントが変更した作業ファイルの一覧を示す **Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、進行中の作業を確認して、必要に応じて方向を調整します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、特定のタスクや分野に合わせた専門的なアシスタントを作成し、チームの知識や標準を再利用できます。

この演習では、次の操作を行いました。

- 既存のアクセシビリティ カスタムエージェントをレビューしました。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装しました。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモードのトグルを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
