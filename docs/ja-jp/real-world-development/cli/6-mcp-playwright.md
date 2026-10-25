---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでも説明したとおり、コードを書くこと以外にもさまざまな作業があります。データや外部サービスを扱い、Copilot が使える追加の自動化機能を用意することもあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot CLI に組み込まれている機能の範囲を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどのように使うかを学びます。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べます。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot に開発中のサイトをユーザーと同じように操作させれば、変更が期待どおりに動作するかを確認しながら、変更作業をさらに自動化できます。

## Model Context Protocol（MCP）とは

[Model Context Protocol（MCP）][mcp-blog-post]を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントはリアルタイムに情報を取得し、最新の情報にアクセスしたり、ユーザーに代わって操作したりできます。

こうしたツールやリソースには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを介してアクセスします。それぞれの MCP サーバーが、AI エージェントの利用できるツールやリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP を見つけやすくし、貢献を促すために[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同じように扱ってください。利用する前にソースコードを確認し、公開者を確かめ、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は追加し、Copilot がユーザーと同じようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Copilot に Playwright で機能を調べてもらう

これまでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に指示します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしてフィルタリングを確認してください。変更は加えず、うまく動作する点としない点を報告してください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は、Copilot に MCP サーバーの使用を明示しなくても、必要に応じて選択します。ただし、使用するツールが分かっている場合は、明示しても問題ありません。作業の一貫性が高まり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行している操作を見ます。
3. レポートを読み、すべてが想定どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作して、サーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで機能を調べるため、Playwright MCP サーバーを使いました。内容を振り返りましょう。

- Model Context Protocol（MCP）とは何か、Copilot CLI がどのように使うかを学びました。
- まだ利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を調べました。

次は、スキルとブラウザーツールを組み合わせた[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
