---
title: "レッスン 4 - カスタムインストラクションで Copilot を導く"
description: "リポジトリのインストラクションを確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを進めたい場合は、そのガイダンスを Copilot が参照できるようにしておきます。[インストラクションファイル][instruction-files]には、必要なコードだけでなく、その構成方法も記述できます。フィルタリング機能を作成したので、Copilot が使ったインストラクションを確認し、ドキュメント標準を追加して、その内容をコードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリのインストラクションとパス固有のインストラクションファイルがエージェントにどう適用されるかを確認します。
- コーディング標準を守るため、インストラクションファイルを更新します。
- インストラクションファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断の理由を記述します。
- `db/` と `src/lib/` のエクスポート関数には、該当する場合の注入可能な `db` 引数も含め、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。
- 再利用可能な Astro コンポーネントには `Props` 契約を記述し、関連するコードが変更されたらコメントを最新の状態に保ちます。
- 既存のフォーマットと lint に関するガイダンスを維持します。

インストラクションファイルを使って、Copilot がこれらの手法に沿ってタスクを実行するための情報を提供します。

## インストラクションファイル

カスタムインストラクションを使うと、コンテキストや設定を Copilot に伝え、コードスタイルや要件を理解しやすくできます。好みのコーディング規則、ライブラリ、コメントの書き方などを指定できるため、提案やコードスニペットを適切なものにするのに役立ちます。リポジトリ全体に適用するインストラクションも、特定のファイル種別に適用するインストラクションも作成できます。

インストラクションファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一のインストラクションファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスク向けのガイドラインを提供します。

> [!NOTE]
> その他のインストラクション形式とサポート状況は、環境によって異なります。特定の形式を利用する前に、[カスタムインストラクションのサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタムインストラクションファイルを確認する

スタータープロジェクトにはインストラクションファイルが含まれています。変更前に既存のファイルを確認し、変更後の影響を比較しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらは、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションがあります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、インストラクションを適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他のインストラクションファイルやプロジェクト内の既存ファイルへのリンクを確認します。大きなインストラクションセットを小さな再利用可能なファイルに分割し、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせてインストラクションファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。`copilot-instructions.md` に TypeScript の新規コードへの TSDoc コメントの追加を指定します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして次の行を追加します。

   ```markdown
   - 新しい TypeScript コードにはすべて、ドキュメント用の TSDoc コメントを記述します。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話開始時にリポジトリのインストラクションを読み込みます。更新したインストラクションを反映するため、会話を終了して再開します。機能のコンテキストは維持されます。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替え、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   インストラクションとコードのガイダンスを更新しました。生成したコードをガイダンスに合うように更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが正確にコードを説明していることを確認します。

## まとめと次のステップ

Copilot CLI がコンテキストをどのように取得するかを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリのインストラクションとパス固有のインストラクションファイルがエージェントにどう適用されるかを確認しました。
- コーディング標準に従うようインストラクションファイルを更新しました。
- インストラクションファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルをカスタマイズして実行し、[lint とテストを一貫して実行できるようにします][next-lesson]。

## リソース

- [Copilot CLI にカスタムインストラクションを追加する][instruction-files]
- [カスタムインストラクションのサポート][custom-instructions-support]
- [効果的なカスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
