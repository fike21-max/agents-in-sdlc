---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルタリングの検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業は、単にコードを書くことだけではありません。コードが動作することを手動で確認し、指示ファイルを使って規約に従うようにしました。では、テストや lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

こうしたタスクには、**エージェントスキル**が適しています。スキルを使うと、これらの操作を適切に実行する方法を Copilot が理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは結果を読みやすくするために、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたものです。エージェントは必要に応じてこれらを読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルで構成されます。この Markdown ファイルには、スキル名と説明を定義するフロントマター、スキルの概要、呼び出すタイミングのガイダンスが含まれます。スキルの呼び出し時に使用するスクリプトやその他のリソースを、サブフォルダーに追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにすでにあるコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリ資産として、プロジェクトの `.github/skills` フォルダーに配置できます。または、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。`description` は Copilot がスキルを呼び出すタイミングを判断するために使われます。
3. 指示を読み、テストと lint のプロセスをどのように案内しているかを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すか、自然言語で依頼して実行できます。スキルに 3 種類のチェックを実行してもらいましょう。

1. Interactive モードでフィルタリングについて会話している画面に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかがわかるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを試してみましょう。Copilot CLI は、会話を再起動しなくても編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最初のレポートと比較し、最後に表示される内容を確認します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の報告形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能の PR に含めます。次は、[Playwright MCP サーバーを通じて][next-lesson] Copilot がサイトを直接操作できるようにします。

## スキルの例

これらのコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
