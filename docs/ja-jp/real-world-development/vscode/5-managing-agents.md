---
title: "演習 5 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

GitHub Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイルの編集、command の実行などを自律的に行います。作業は手元のマシン上でリアルタイムに進むため、VS Code の Copilot Chat では tool の呼び出しやファイルの編集を逐次確認できます。diff をその場でレビューし、変更ごとに承認または取り消しを行い、追加の prompt で会話の方向を調整または作業を拡張できます。エディターを離れる必要はありません。

この演習では、次のことを行います。

- アクセシビリティ エージェントの作業をリアルタイムで監視します。
- 提案されたファイル編集をレビューして承認します。
- session の途中でエージェントに指示を追加し、ハイ コントラストに加えてライト モードの toggle を作成します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ カスタム エージェントは、ハイ コントラスト モードを実装中です。作業中にデザイン チームから追加の依頼がありました。既定のテーマが暗すぎると感じるユーザー向けに、ライト モードの toggle も追加してほしいとのことです。新しい会話を最初から始める代わりに、現在の session を使って作業を追加します。

## Copilot の進捗を監視する

Copilot Chat ビューには、エージェントが実行する各ステップが表示されます。調査するファイル、実行する command、提案する編集を確認できます。各ファイルの編集は diff と **Keep** / **Undo** の操作とともに chat に表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. エージェントの作業中に chat をスクロールし、tool の呼び出しを確認します。ファイルの読み取り、編集、実行許可を求めるターミナル command が表示されます。
3. エージェントがファイルの編集を提案すると、diff が chat に表示されます。
    - **Keep** を選択すると変更を承認します。
    - **Undo** を選択すると変更を取り消し、エージェントにもう一度試すよう求めます。
4. chat の上部にある **Files changed** の表示を開くと、この session でエージェントが変更したファイル全体を確認できます。

> [!NOTE]
> tool の呼び出し（たとえばターミナル command の実行）に許可が必要な場合、エージェントは一時停止して **Allow** を表示します。許可するのは、実行しても問題ない command のみにしてください。

5. エージェントがハイ コントラスト モードを実装し終えるまで待ちます。完了すると、行った変更の概要が表示されます。

## ライト モードを追加するようエージェントに指示する

ハイ コントラスト モードが完成したので、同じ会話でライト モードの toggle を追加します。同じ chat session を続けると、変更したコンポーネント、使用した style の方法、設定した local storage の pattern など、前の作業のコンテキストをエージェントが保持できます。

1. Copilot Chat の入力欄に次の follow-up prompt を送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. エージェントは、すでに行った変更を確認してからライト モードの追加を提案します。
3. 変更が提案されるたびにレビューします。diff ごとに **Keep** または **Undo** を使います。
4. エージェントの進め方が希望と異なる場合（たとえば toggle UI のリファクタリング方法が意図と違う場合）は、chat 入力欄の横にある **Stop** ボタンで停止します。その後、意図を明確にする follow-up message を送信します。

> [!TIP]
> 作業中の session に指示を追加すると、最初からやり直すより早く調整できます。follow-up prompt を使い、「CSS を簡潔にし、selector を重複させず CSS variable を使う」のような変更や、「toggle を 3 つにせず、ライト モードを既存の設定パネルに統合する」のような軌道修正、「両方の toggle に keyboard shortcut も追加する」のような範囲の拡張を依頼できます。

## 作業中の変更をレビューする

commit する前に、エージェントが変更した内容をひととおり確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro コンポーネント、style、関連する test が更新されているはずです。
3. 変更されたファイルをいくつか開いて diff を確認します。カスタム エージェントのアクセシビリティの方針が反映されていること（ARIA 属性、keyboard navigation、semantic HTML、local storage による設定の保持）を確かめます。
4. 問題がなければ、Source Control パネルで変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

このレッスンでは、Copilot Chat の agent mode を使い、進行中の session を監視して方向を調整しました。ファイル編集を確認しながら承認または取り消しを行い、コンテキストを保ったまま follow-up prompt で作業を拡張しました。

このレッスンで確認した内容は次のとおりです。

- エージェントの作業をリアルタイムで監視する。
- 個々のファイル編集を承認または取り消す。
- 作業中の session に follow-up prompt を送って方向を調整する。

[次のレッスン][next-lesson]では、アクセシビリティ作業を pull request として公開し、このラボでローカルのカスタム エージェントが生成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode を使う][agent-mode]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
