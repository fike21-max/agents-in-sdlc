---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業をより効果的に行えるように、焦点を絞ったインストラクション、ベストプラクティス、コーディングパターン、専門分野の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag]準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門性、インストラクションを定義する Markdown のプロンプトが必要です。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、ある程度の重なりがあります。どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。簡潔に区別すると、**カスタムエージェントは作業者で、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一環としてスキル（さらには他のエージェント）を調整できます。このラボでは、アクセシビリティのカスタムエージェントがアクセシビリティガイドラインに照らしてサイトをレビューし、必要に応じて更新します。その作業の一部として、プルリクエストのワークフロー用やテスト実行・管理用のスキルを呼び出すことができます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが十分でなく、読みづらいというフィードバックがありました。このアクセシビリティの課題に対処するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、この機能をできるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。Copilot の動作をどのように導くか、内容を確認しましょう。

codespace に戻り、アクセシビリティのカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルなウェブサイトのコードを生成する際の主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と間違い。

## issue を作成して割り当てる

ミッションコントロールは、環境内のすべてのエージェントと作業を管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を送れます。まず Copilot にハイコントラストモードの作成を依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して新しい issue を作成します。
5. **Title** に `ウェブサイトにハイコントラストモードを追加する` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトにハイコントラストモードが必要です。ユーザーが設定できるハイコントラスト切り替え機能を追加してください。設定はブラウザーの local storage に保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![クラウドエージェント割り当て画面で、custom agent と accessibility が強調表示されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業をより効果的に行えるように導けます。

この演習では、次のことを学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方法。

Copilot がハイコントラストモードの実装を進めている間に、次はミッションコントロールから[エージェントセッションを監視して管理します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
