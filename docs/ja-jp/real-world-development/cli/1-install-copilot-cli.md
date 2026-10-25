---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されたフィルタリングの issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンドラインから行えるため、タスクを任せて変更を依頼しながら、作業に集中できます。まずはツールをインストールしましょう。すでに使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 表示された場合は、デバイスコードを使う手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys のリポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語がエージェントに送られ、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストールされているバージョンで使えるコマンドを確認します。次に <kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが使えることを確認します。
6. Copilot にフィルタリングの issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の変更を終えた後にこの issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使い、自動的には worktree を作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの権限の範囲外でツールを使う前に確認します。このワークショップでは、Codespace 内の承認プロンプトを省略して演習に集中できるよう、ユーザーが承認するショートカットの `--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は、完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常的な開発の既定値として alias に設定しないでください。詳しくは[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を付けると、後のレッスンで issue やプルリクエストを操作するために使う、読み取り / 書き込み用の GitHub MCP ツールが有効になります。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更を確認してください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて別の簡単な質問をし、会話が動作することを確認してから、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングの issue を見つけました。

次は、[最初の変更を行い][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
