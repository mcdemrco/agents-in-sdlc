---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、エージェントに焦点を絞った指示、ベストプラクティス、コーディング パターン、分野固有の知識を与えます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、テスト パターンの一貫性を保つテスト エージェントなど、チームの知識を再利用できるエージェントに反映できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー全体に適用する `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを編成して作業できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローを扱うスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次の操作を行います。

- カスタムエージェントの定義方法を確認します。
- エージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるよう取り組んでいます。現在のダークテーマは、テキストと背景色のコントラストが不十分で、読みにくいという意見がユーザーから寄せられました。デザイン チームは、ユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、この機能をすばやく実装します。カスタムエージェントに機能の生成を依頼します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認し、Copilot を導く指示を理解します。

アクセシビリティ カスタムエージェントをレビューするため、Codespace に戻ってファイルを開きます。

1. Codespace に戻ります。
2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

ミッション コントロールは、環境内のすべてのエージェントを扱う中心的な場所です。Copilot cloud agent へのタスクの割り当て、進捗の監視、作業中のセッションへの追加指示ができます。まず、ハイコントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

   ```plaintext
   サイトにハイコントラスト モードを追加してください。ユーザーが切り替えられるトグルを用意し、ブラウザーの local storage に設定を保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントとアクセシビリティ エージェントを選択した cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、特定のタスクや分野に合わせた専門的なアシスタントを作成し、チームの知識や標準を再利用できます。

この演習では、次の操作を行いました。

- カスタムエージェントの定義方法を確認しました。
- エージェントにタスクを割り当てました。

Copilot がハイコントラスト モードを実装している間に、次はミッション コントロールから[エージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
