---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、何を実装するかだけでなく、どのように実装するかも Copilot に伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、この演習では同じプロンプトの実行前と実行後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、影響をリアルタイムに並べて示すのが難しいためです。後続の演習で cloud agent が作成したプルリクエスト (PR) をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ層には必ずユニットテストを用意します。
- ユーザー インターフェイス (UI) はダークモードにし、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを使います。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に伝えられます。コーディング規約、ライブラリ、コードコメントの方針を指定することで、より適切な提案を得られます。リポジトリ全体、または特定のファイルの種類に指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。技術スタック、開発内容の概要、ベストプラクティスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニットテストなどのタスクに関する指針を記述します。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法はひとつではありません。サンプル プロジェクトのファイルを参考に、必要なガイダンスを整えます。

- `copilot-instructions.md` には、作成するもの、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関する指針を記述します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに対する指針に使います。
- 自然な言葉を使い、明確なガイダンスとコードの良い例・悪い例を示します。

AI の使い方に唯一の正解がないように、指示ファイルの作成方法にも唯一の正解はありません。プロジェクトに合う方法を試して見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには [UI の指示][ui-instructions]や [Astro の指示][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、操作は環境によって異なります。必要な場合は該当する演習で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` が用意されています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針も確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示が適用されるファイルを表す glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクト ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。ここにあるパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイル先頭のコメントの規則を追加します。

## リポジトリ全体の標準を追加する

ここで行う編集は、cloud agent が後の演習で割り当てられた issue に取り組むときに参照する、TSDoc コメントとファイル先頭のコメントの標準です。

1. Codespace のターミナルで次のコマンドを実行し、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えます。コードの提案を改善するために、リポジトリのコーディング標準を記述します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準が記載されていますが、ドキュメント コメントの規則はまだありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル先頭のコメントと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の標準には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使います。ドキュメント コメントの規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記述します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、作業対象の issue が指定するブランチから指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が新しい指示を利用できるように、更新を `main` にマージします。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue を割り当てる際、cloud agent が更新された指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成する手順を省略し、`main` にコミットします。ここでは実際のプロジェクトに近い手順を説明しています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体へ適用される標準を追加しました。cloud agent がこの環境で作成するプルリクエストをレビューするときに、追加した標準が反映されます。

次は[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
