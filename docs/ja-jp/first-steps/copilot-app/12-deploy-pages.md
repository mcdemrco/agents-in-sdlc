---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages にデプロイして、Space Quiz を一般公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages のデプロイ設定を依頼します。
- マージする前にワークフローとプルリクエストをレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる場所がありません。GitHub Pages を使えば、サーバーや実行時依存関係を追加せず、公開先の Uniform Resource Locator (URL) からサイトにアクセスできます。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
このリポジトリで GitHub Pages を有効にしてください。必要に応じて Web サイトをビルドし、GitHub Pages にデプロイする GitHub Actions ワークフローを作成してください。README がまだない場合は、アプリの説明とローカルでの実行方法を記載した README を追加してください。これらの変更を含むプルリクエストを作成し、必須チェックが成功した後にマージしてください。デプロイが成功するまで監視し、公開サイトの最終 URL を報告してください。Pages の有効化やマージに必要な権限がない場合は、私が何を承認または実行する必要があるかを具体的に教えてください。サイトが公開されるまで、成功したと報告しないでください。
```

![マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の実行、yourname.github.io/space-quiz/ で公開された Space Quiz の3段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。作業を進める前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がまだない場合に限り、README が追加されていることを確認します。
2. ビルドとデプロイの手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、提示された具体的な手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで、作業が完了したと判断しないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らず、実際にサイトを確認しました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
