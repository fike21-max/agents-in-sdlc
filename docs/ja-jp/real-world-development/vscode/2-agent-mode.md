---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、test を実行する必要があります。開発者の作業では、必要なファイルを探し、変更を加え、test を実行し、問題をデバッグし、更新し忘れたファイルを見つけ、再び変更する、といった作業が続きます。

こうした作業に役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業できるよう設計されています。開発者と同じように、既存のプロジェクト構造を調べ、必要な更新を行い、test などのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新機能を追加してみましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる体験を提供します。agent mode は IDE 内で作業し、すぐにフィードバックを得てやり取りできます。cloud agent はチームの一員のように非同期で作業する peer programmer で、issue や pull request を通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う方法。
- GitHub Copilot Agent Mode で UI とデータ層に新機能を実装する方法。
- Copilot Agent Mode が生成した変更と test を確認し、コードベースに merge する前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや publisher で絞り込めるようにします。前の演習では publisher 用の helper を追加しました。この演習では、Copilot Agent Mode を使ってデータ層、UI、test に残りの作業を行います。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro app で、build 時にページを静的 HTML としてレンダリングします。ページの frontmatter で Drizzle ORM を介してローカルの SQLite データベースを直接クエリします。独立した backend API や client-side UI framework はありません。再利用可能なデータ アクセス helper は `src/lib/` に配置します。インタラクティブな機能は、標準の DOM API を使った小規模な Astro `<script>` で実装します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro dev server を起動できます。次の手順で GitHub Codespace 上で実行します。

1. codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内で新しいターミナル ウィンドウを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次の command を実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL を含む banner が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログ ボックスが表示された場合は、**x** を選択して閉じてください。

5. ターミナルで Astro server のアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してください。

> [!NOTE]
> codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。codespace へのプライベート tunnel を介して Web server にアクセスできます。

### Web サイトを確認する

Web サイトを起動したら、機能を確認しましょう。主な機能は次のとおりです。

- **Home Page**: ボード ゲームのタイトル、画像、説明の一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定したときに issue のバックログが自動作成されました。GitHub Copilot に内容を表示してもらいます。

1. Chat ビューの agent ドロップダウンから **Agent** を選択します。**Agent** は自律的に計画し、複数ファイルの変更を実装して、ターミナル command を実行し、ツールを呼び出します。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。快適に進められるよう、受け取るコードの提案が比較的一貫することを期待しています。ただし、LLM の出力は確率的なため、ワークショップに記載されたものと異なる提案が出ることがあります。これは正常な動作です。

> [!NOTE]
> LLM の出力は確率的なため、Copilot が別の MCP command を使う場合があります。その場合もタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、GitHub repository の issue バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. issue をすべて一覧表示する command を実行するには、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって最も役立つと思われる項目を基に、優先順位も付けていることを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝える指示ファイルを確認しましょう。Web サイトに機能を追加する方法に関するコンテキストを含む [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加するための全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、test 可能性、アクセシビリティに関する原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用したり、大きな指示を複数の小さなファイルに分割したり、Copilot が参照できる例やテンプレートを指定したりできます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データ層 (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリング動作の test を追加または更新する。
- ゲーム一覧ページを更新し、フィルタリング UI を追加する。

さらに、コードベースに merge する前に test を実行し、成功することを確認します。Copilot Agent Mode にこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue の要件に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを見つける様子を確認します。データ層の helper、UI、test を見つけてファイルの変更を開始します。

   ![Copilot がプロジェクトのファイルを調べている様子のスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、test の実行などを行います。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中に、コード エディターに **Keep** と **Undo** が表示されることがあります。Copilot が完了するとすべての変更に対する **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択して test を実行します。

   ![Copilot Chat ペインで test 実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業の途中で一時停止したり、test が失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーが検出されなくなるまで、コード生成と test の間を行き来します。

   ![Copilot Agent Mode との Chat セッション完了画面のスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やその他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行しているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作することを確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新機能を追加しました。次の方法を学びました。

- GitHub Copilot Agent Mode で UI とデータ層に新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う方法。
- Copilot Agent Mode が生成した変更と test を確認し、コードベースに merge する前にレビューする方法。

次は [Playwright MCP server で機能をテスト][next-lesson]し、pull request を作成します。

### 発展演習 – paging を実装する

ゲームの一覧が増えると、paging が必要になります。この演習で学んだことを使って、Copilot に paging を実装するよう依頼してください。次の点を検討しましょう。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、固定値にするかなど、paging の実装方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを追加しながら何度かやり取りする場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使いどころ][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
