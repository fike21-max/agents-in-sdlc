---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]もその 1 つです。このミッションコントロールでは、開いているプルリクエスト（PR）があるエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェントページを使って cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイルと切り替え機能を更新しているため、両方を実装するのが合理的です。エージェントの作業に指示を追加し、ハイコントラストモードに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左側のペインにあるタスク一覧を確認します。Copilot に割り当てた次のタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更するための API を生成する。
   - ウェブサイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択します。Copilot が行った次のような操作を確認できます。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した進め方によって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更されたファイルを確認し、さらに詳しく監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページのタスクで、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が完了すると、追加の依頼が作業に取り込まれます。

前と同じように、完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページで、Copilot cloud agent のタスクを監視して案内する方法を学びました。このミッションコントロールでは、実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクに追加の指示を送ることができます。

このレッスンでは、次のことを行いました。

- cloud agent のタスクを監視するために、ミッションコントロールとエージェントページを確認しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを反復的に改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
