---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request を作成する前に、ブラウザーで実際に動作することを確認しましょう。アプリを自分で操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP server を workspace に追加します。
- Copilot に Playwright MCP server を使ってブラウザーでフィルタリング機能を手動テストするよう依頼します。
- ブランチを公開し、フィルタリング作業の pull request を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外にあるツールに接続すると、さらに多くの作業ができるようになります。Copilot が外部ツールに接続するための標準が Model Context Protocol (MCP) です。

![agent mode の内部構造と、コンテキスト、LLM、ツール（MCP server や VS Code 拡張機能が提供するツールを含む）との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI agent は外部ツールやサービスと通信できます。MCP によって、AI agent は外部ツールやサービスとリアルタイムに通信できます。これにより、最新情報（resource）にアクセスしたり、ユーザーに代わって操作（tool）を実行したりできます。

これらの tool や resource は、AI agent と外部ツールやサービスをつなぐ MCP server を介してアクセスします。MCP server は AI agent と外部ツール（既存の API や NPM package などのローカル ツール）の通信を管理します。各 MCP server は、AI agent が利用できる異なる tool と resource のセットを提供します。

よく利用されている MCP server の例を 2 つ紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository を管理する API にアクセスできます。AI agent に repository の作成や更新、issue や pull request の管理などを実行させることができます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI agent に Web ページへの移動、フォームへの入力、ボタンの選択などを実行させることができます。

ほかにも、さまざまな tool や resource を提供する MCP server があります。エコシステム内の MCP server を見つけやすくし、貢献できるよう、GitHub では [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの面では、MCP server もほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、公開元を検証して、セキュリティ上の影響を検討します。信頼できる MCP server のみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルでは、この VS Code workspace で利用できる MCP server を設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` server があらかじめ設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が issue のバックログを読み取れました。[remote GitHub MCP server][remote-github-mcp-server] を使うため、ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot が初めて tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が操作できるブラウザーが利用できます。これは機能のテストに役立ちます。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

remote の `github` server とは異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag を指定すると、Playwright は画面にウィンドウを表示せずにブラウザーを実行します。codespace には表示用のデスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは end-to-end test に Playwright を使っているため、通常、必要な Playwright browser はすでにインストールされています。Copilot が後で browser が見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼してから、もう一度試してください。

## server を起動して信頼する

VS Code は必要になったときに MCP server を起動します。Copilot が server の tool を初めて必要とすると、VS Code が server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** action が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. VS Code から server を信頼するか確認されたら、設定を確認して信頼するよう選択し、server を起動します。

手動で起動する手順は省略できます。その場合、次の手順で Copilot が初めて tool を必要としたときに、VS Code が server を起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際の browser を操作できます。自分でアプリを操作して確認する代わりに、agent にページを開き、移動し、filter を適用して結果を読み取らせ、その内容をまとめてもらえます。会話を離れずに、機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP server は、スクリーンショットではなくページの [accessibility tree][playwright-mcp-server] を使います。つまり、agent は button、link、list item など、ラベル付きの構造化された要素を基に判断します。支援技術と同じように情報を読み取るため、簡単な機能確認をアクセシビリティの基本チェックとしても活用できます。

server が接続され、アプリが実行中であることを確認したら、次のプロンプトで Copilot にフィルタリング機能をテストさせます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由で browser を起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らし合わせて内容を確認してください。想定と異なる点があれば、pull request を作成する前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> この test にはアプリが `http://localhost:4321` で実行されている必要があります。dev server を停止した場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP server を初めて使うときに browser のダウンロードが必要な場合があります。browser がないと報告されたら、`npx playwright install chromium` を実行するよう依頼してから、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたら、チームのレビュー用に pull request (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを選択して commit message を生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## pull request を作成する

github.com や GitHub command-line interface (CLI) などを使って pull request を作成できます。今回は GitHub Copilot に作成を依頼します。関連する issue を見つけ、その issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. GitHub の tool を使えるよう、agent ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のように Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために必要な場合は、**Continue** を選択します。GitHub の tool を初めて使うときに GitHub 認証を求められたら、画面の指示に従って許可してください。
5. Copilot が issue を検索し、該当する issue を見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して pull request を確認します。ただし、まだ **merge しないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot に新しい tool を追加する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- Playwright MCP server を使い、機能を公開する前に browser でフィルタリング機能をテストしました。
- ブランチを公開し、フィルタリング作業の pull request を作成しました。

MCP server を使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタム エージェントを作成][next-lesson]し、アクセシビリティのレビューなど、範囲を絞った作業を効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
