---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムインストラクション][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな問題があります。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできる下書きのプルリクエスト（PR）を作成します。面倒なタスクを任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 明確な範囲設定が issue にとって重要な理由を知る。
- Copilot に issue を割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した契約業者は、ドキュメントを適切に残していません。実際に、ドキュメントがほとんどありません。まず、アプリケーション内のすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリの別のタイトルを表示して、閲覧を続けられるようにしたいとのことです。完成度の高い実装はまだ必要ありません。受け入れテストで UX を確認できれば十分です。これは現在ブロッカーですが、優先度がさらに高い issue もあります。

どちらも、優先度を下げられがちなタスクの例です。Copilot cloud agent に割り当てれば、開発者が別の作業に集中している間、非同期で進められます。その後、Copilot の作業をレビューして、期待どおりかを確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡する下書きのプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中でも完了後でも Copilot のセッションを確認できます。提案された解決策がレビューできる状態になると、プルリクエストであなたにメンションします。

## 適切な範囲を持つ指示が重要な理由

GitHub Copilot に魔法はありません。考える手間を省くことはできません。一見単純な操作でも、詳細を確認すると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を適切に設定してください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタムインストラクション

これまでの演習で、カスタムインストラクションと Copilot のガイド方法を学びました。カスタムインストラクションの演習を終えている場合は、`.github/copilot-instructions.md` がどう適用されるかを確認しています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれるインストラクションファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの GitHub ウェブ UI または、すでに起動している codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規則と TSDoc のドキュメントコメントに関する要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに適用されることを確認します。

次のセクションで `コードのドキュメントが不足している` issue を cloud agent に割り当てます。作成されたプルリクエストで、これらのインストラクションに由来する TSDoc のドキュメントコメント、TypeScript の規則、コメントヘッダーを確認してください。後の演習で PR をレビューするときにも、改めて説明します。

## Copilot cloud agent 用の開発環境をセットアップする

誰がコードを作成する場合でも、通常は開発環境を準備し、必要なセットアップスクリプトを実行して状態を整えます。ソフトウェアエンジニアと同じように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions]を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用セットアップワークフロー][setup-workflow]を作成すると、作業開始前に環境をカスタマイズし、必要な開発ツールや依存関係を利用できるようにできます。ラボをスムーズに進め、この機会に学べるよう、事前に設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリのマイグレーション済み・シード済み SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできるようにします。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` が必要です。セットアップ完了前にクローンしない場合、Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: コードをチェックアウト
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体が Astro + Drizzle/libSQL です）
      - name: Node.js をセットアップ
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: JavaScript の依存関係をインストール
        run: npm ci

      - name: Playwright ブラウザーをインストール
        run: npx playwright install --with-deps chromium

      # 単一 Astro アプリのビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: データベースをセットアップ
        run: npm run db:setup
```

このファイルは通常の GitHub ワークフローに似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使っています。リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正しく動作するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメントがなかったりします。こうした技術的負債は放置されやすく、生産性を低下させ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で、その結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して新しい issue を作成します。
5. **Title** に `コードのドキュメントが不足している` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   関数やメソッドには、役立つ場合に TSDoc のドキュメントコメントを含めるという組織の要件があります。しかし、最近の更新でこの標準が守られていません。動作を明確にするコメントが必要な既存コードに、ドキュメントコメントを追加する必要があります。
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: コードのドキュメントが不足している` のようになります。PR が一覧に表示されない場合は、少し待ってブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し（前述のワークフローを実行）、プロジェクトを分析して、タスクを進める方法を検討しています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されていない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールして、Copilot が問題をどのように解決しているかを確認します。コードと現在の状態の調査、計画を考えるための一時停止、コード作成などが行われます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを実行し、開発者が別の作業に集中できるようにすることです。次は、もう 1 つのタスクを Copilot cloud agent に割り当て、その間にアプリケーションへ機能を追加するコードを書いてみましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の 1 つは作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業が短時間で終わるとしても、その間に別のタスクを進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `ゲーム詳細ページに関連ゲームを表示する` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   ゲーム詳細ページに各ゲームの関連ゲームを表示して、さらに多くのゲームを見つけやすくしたいと考えています。デザインチームは UX を検討し、受け入れテストを行う予定です。要件は次のとおりです。

   - `src/lib/` に、指定したゲームと同じカテゴリに属する別のゲームを返すデータアクセスヘルパーを追加する。指定したゲーム自体は除外する。
   - ゲーム詳細ページにヘルパーを使った「関連ゲーム」セクションを表示する。
   - 関連ゲームがない場合に対応する。
   - 新しいヘルパーのユニットテストを作成する。
   - PR を作成する前に、すべてのテストが成功することを確認する。
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、積極的に監視する必要はありません。完了したらレビューできます。それでは、カスタムエージェントの作成と利用について確認しましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期に作業を任せられます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを依頼できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- issue の範囲を明確に設定する重要性を学びました。
- Copilot に issue を割り当てました。

Cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と利用を確認します。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタムインストラクションも参照します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
