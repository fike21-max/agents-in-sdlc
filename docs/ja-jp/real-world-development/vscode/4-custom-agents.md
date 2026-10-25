---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI assistant を作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、エージェントが特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、専門知識を与えます。チームの知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫した test パターンを維持する test エージェントなどがあります。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を指定した YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown prompt が含まれます。

### カスタム エージェントと agent skills の比較

カスタム エージェントと [agent skills][agent-skills-concept] には、ある程度の共通点があります。どちらも主に Markdown ファイルで定義され、AI が操作を実行する方法を伝えます。違いを簡潔に表すと、**カスタム エージェントは作業者、skills はツール**です。

カスタム エージェントには独自の context window があり、作業の一部として skills（場合によっては他の agent）を調整して実行できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、pull request workflow skill や test を実行・管理する skill などを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー feedback から、テキストと背景色のコントラストが不十分なため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティ上の課題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、あらかじめ作成されています。Copilot をどのようにガイドするか理解するため、内容を確認しましょう。

codespace に戻り、ターミナルを開きます。次の command を実行して、アクセシビリティ作業用に `main` から新しいブランチを作成します。フィルタリング用の PR は演習 3 で作成したため、別のブランチで作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタム エージェントが、Copilot Chat ビュー下部の agent ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの指示と tool に範囲を限定した chat session を開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューの agent ドロップダウンで **Agent** が選択されていることを確認します。未選択の場合は選択してください。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. chat ビューの下部にある agent ドロップダウンを選択します。現在の agent が表示されています。既定では **default** です。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. 次の prompt をアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集内容はファイル パスと diff とともに chat に表示され、確認できます。
6. 編集が追加されると、**Files changed** の表示が更新され、この session でエージェントが変更したファイルが反映されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連する test など、実際の repository のファイルを変更します。

次の演習では、作業中の変更をレビューして方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI assistant である[カスタム エージェント][custom-agents]について学びました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、Copilot が特定の作業をより効果的に行えるようにガイドできます。

次の内容を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は[エージェントの作業を監視して方向を調整][next-lesson]します。変更が進む間にレビューし、同じ session でライト モードの toggle を追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの設定][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
