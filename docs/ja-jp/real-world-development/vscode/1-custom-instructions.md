---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。重要な方法のひとつが[指示ファイル][instruction-files]です。指示ファイルでは、コード生成の指針を伝えます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルタリング用の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ共通の標準を追加します。
- 同じプロンプトを再度実行し、生成コードに新しい標準が反映される様子を確認します。
- 指示の更新とフィルタリングの実装を `main` にコミットし、Copilot が次の演習で更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどのように変わるかを確認します。生成結果が文字単位で同じになることは期待しないでください。

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

どのプロジェクトにも当てはまる唯一の方法はありません。試しながら、プロジェクトに合う方法を見つけてください。

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
2. プロジェクトの簡潔な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針も確認します。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示が適用されるファイルを表す glob を指定しています。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクト ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイル先頭のコメントの規則を追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な手順は各演習で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルのままプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を返すデータアクセス ヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパーとテストの両方が変更されることがあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 生成されたコードに TSDoc コメントとファイル先頭のコメント ブロックが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが重要です。

## リポジトリ全体の標準を追加する

`.github/copilot-instructions.md` は、プロジェクト全体に関する情報を Copilot に伝えるためのファイルです。コードの提案を改善するために、コーディング標準を記述します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング標準が記載されていますが、コード内のドキュメントの規則はまだありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル先頭のコメントと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の標準には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使います。ドキュメント コメントの規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記述します。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各パブリッシャーの名前と ID を返すデータアクセス ヘルパーを作成または更新してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのファイルと同じように、ソース管理で管理します。指示とヘルパーの変更をリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎部分として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエスト (PR) を作成してマージする

プッシュしたブランチから PR を作成し、バックログのドキュメント標準に関する issue と関連付けます。GitHub ツールはプロジェクトに接続済みなので、Copilot に issue を検索させ、issue をクローズする PR を作成して、マージを依頼できます。

> [!NOTE]
> GitHub で Copilot に操作を依頼するのは今回が初めてです。プロジェクト テンプレートには GitHub ツールがすでに設定されています。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。表示される手順に従ってください。次の Model Context Protocol (MCP) の演習では、この接続のしくみを確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選ばれていることを確認します。
3. 次のプロンプトを送り、指示ファイルの更新と新しい publishers ヘルパーの基礎部分を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```plaintext
   指示ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その issue をクローズすることと、今後のフィルタリング作業に向けて publishers ヘルパーの基礎部分も追加したことを記載してください。
   ```

4. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次のプロンプトを送り、PR をマージしてローカルのブランチを `main` に戻し、最新のコードを取得するよう依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新の状態にしてください。
   ```

6. Copilot が GitHub 操作やシェル コマンドの実行許可を求めたら、内容を確認して適切な場合に許可します。

GitHub Copilot の支援で PR を作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- リポジトリのカスタム指示とパスに応じた指示ファイルが Copilot に適用される方法を確認しました。
- 現在の指示を使ってフィルタリング用の publishers ヘルパーを生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再度送り、更新した標準が提案コードに反映される様子を確認しました。
- 指示の更新とフィルタリングの一部を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]を使い、Copilot にコードベース全体にわたる機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
