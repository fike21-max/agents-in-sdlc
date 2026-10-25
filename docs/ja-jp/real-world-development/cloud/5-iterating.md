---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験を改善し、機能を追加するいくつかのタスクに GitHub Copilot を使いました。コードへのドキュメント追加、デザインチームが改善に使う関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善しましょう。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。AI が生成したコードを必ずレビューし、通常の DevOps プロセスに沿って進めます。チームによるレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ人の監視なしにタスクを実行するため、安全を確保するためのセキュリティ制限があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードの作業に使うブランチに限って書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用に個別の一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人による承認が必要です。
- [外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト（PR）、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、この表示は**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエスト一覧を開きます。
3. **ドキュメント不足の修正**、または同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、コードに新しく追加されたドキュメントを確認します。変更内容の詳細は実行ごとに異なります。

   変更を確認する際は、演習 2 の最初に確認したカスタムインストラクションファイルに基づく TSDoc のドキュメントコメント、TypeScript の規則、コメントヘッダーを探します。

6. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. 一部のワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションで、ワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するとき、やり取りは一方通行ではありません。チームメンバーと同じように、PR のコメントやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを受け取り、対応する新しいセッションを開始します。結果は決定的ではないため、指示の定型文はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例、またはコードベースのドキュメントについて Copilot に依頼したい別の内容を使って、**@copilot** にメンションするコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足できる結果になるまで、引き続き改善を依頼できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **ゲーム詳細ページに関連ゲームを表示する**、または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. 一部のワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェックセクションで、ワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**）。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同じように、Copilot cloud agent に改善を依頼できます。たとえば、表示する関連ゲームの数を変更したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（これはカスタムインストラクションの更新前に割り当てたタスクであることを思い出してください）。これまでと同様に、**Conversation** タブでコメントを追加すると Copilot がそれを受け取り、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **ウェブサイトにハイコントラストモードを追加する**、または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用のトグル UI コンポーネント
   - ユーザー設定を保存する local storage の利用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認し問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. 一部のワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションで、ワークフローがキューに追加されたことを確認します。すべて順調に進めば、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従って機能を実装するよう Copilot を導いたことが分かります。アクセシビリティ上の懸念や改善点があれば、これまでの PR と同様にコメントで **@copilot** にメンションして変更を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて、ほかの作業に集中できるときに役立ちます。習慣を身につけるため、リポジトリに issue をいくつか作成して Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のワークショップを完了しました。これらのレッスンを通して、次のことを行いました。

- **このリポジトリに含まれるカスタムインストラクションファイルを確認し、cloud agent の出力への影響を後から確かめられるようにしました。**
- **issue を Copilot cloud agent に割り当て、環境をセットアップし、計画を立て、非同期に作業する様子を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストモードとライトモードの切り替えを追加しました。**
- **Copilot agents ページをミッションコントロールとして使い、進行中のアクセシビリティセッションを監視して指示を送りました。**
- **cloud agent のプルリクエストをレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent のワークショップを完了しました。さらに学習したい場合は、次のワークショップで今回の内容を補完できます。

- 🖥️ **[VS Code ワークショップ](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ワークショップ](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 機能を使って、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のようなことを試してみてください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
