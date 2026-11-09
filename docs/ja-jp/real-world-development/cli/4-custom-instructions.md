---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も指定できます。フィルタリング機能を作成したので、Copilot が使用した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントには、コードの繰り返しではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入する場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変更された場合はコメントも更新します。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に伝えられます。ライブラリや規約、コードコメントの方針も指定でき、提案の精度を高められます。リポジトリ全体、または特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記述します。

> [!NOTE]
> ほかの形式の指示ファイルや各環境でのサポート状況は異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更による影響を確認できるよう、まずスタータープロジェクトに用意された指示ファイルを確認します。

1. Codespace に戻ります。
2. ターミナルではなく、Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示が適用されるファイルを表す glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ不足があります。`copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 次の内容を、このセクションの最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリング機能のコンテキストを保ったまま、新しい指示を読み込ませるために会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のプロンプトを送り、更新したガイダンスを適用します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードと一致していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルをどのように読み込むかを確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
