---
title: "演習 1 - カスタムインストラクション (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に背景情報を把握してほしい場合は、そのコンテキストを参照できるようにしておきます。[インストラクションファイル][instruction-files]を使うと、Copilot に必要な作業だけでなく、その進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタムインストラクションとパス固有のインストラクションファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のワークショップとは異なり、この演習ではプロンプトの**変更前と変更後**を比較しません。Copilot cloud agent は GitHub issue に対して非同期で動作するため、リアルタイムで影響を比較するのが難しいためです。このワークショップの後半で cloud agent が作成したプルリクエストをレビューするときに、インストラクションファイルの影響を確認します。

## インストラクションファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、常にユニットテストが必要です。
- UI はダークモードにし、モダンな印象にします。
- コードに TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

インストラクションファイルを使って、Copilot がこれらの手法に沿ってタスクを実行するための情報を提供します。

### カスタムインストラクション

カスタムインストラクションを使うと、コンテキストや設定を Copilot に伝え、コードスタイルや要件を理解しやすくできます。好みのコーディング規則、ライブラリ、コメントの書き方などを指定できるため、提案やコードスニペットを適切なものにするのに役立ちます。リポジトリ全体に適用するインストラクションも、特定のファイル種別に適用するインストラクションも作成できます。

インストラクションファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一のインストラクションファイルです。Copilot に送信されるほとんどのチャットや CLI リクエストに関連する、プロジェクト全体の情報を記述します。技術スタック、開発中のものの概要、ベストプラクティス、全体的なガイダンスなどを含められます。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に作成できます。TypeScript や Astro などの言語や、UI コンポーネントや新しいユニットテストの作成などのタスクについて、ガイドラインを提供できます。

> [!NOTE]
> IDE で作業する場合、インストラクションファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルのインストラクションと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準のインストラクションファイルにも対応しています。[カスタムインストラクションのサポート][custom-instructions-support]を参照してください。

### インストラクションファイルを管理するためのベストプラクティス

インストラクションファイルの作成に関する詳細は、このワークショップの対象外です。ただし、サンプルプロジェクトには代表的な方法が含まれています。基本的なポイントは次のとおりです。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなど、特定のファイル形式やタスクのガイダンスに使います。
- 自然な言葉で記述します。ガイダンスを明確にし、コードの適切な例と不適切な例を示します。

AI の使い方に唯一の方法がないのと同じく、インストラクションファイルの作り方にも唯一の方法はありません。プロジェクトに最適な方法を、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実したインストラクションファイルを用意してください。このプロジェクトのインストラクションを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot にインストラクションファイルの生成を依頼することもできます。操作方法は環境によって異なります（VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など）。現在の環境に対応するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、インストラクションファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタムインストラクションファイルを確認する

このリポジトリには、基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub のウェブ UI で開いて確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのやり取り全体に適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどのインストラクションを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. このプロジェクトのユニットテスト作成に関するインストラクションを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、最後までスクロールします。`unit-tests.instructions.md` などのインストラクションファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きなインストラクションセットを小さな再利用可能なファイルに分け、Copilot が参考にできる例を示せます（リンク先のパスは、リポジトリのルートではなくインストラクションファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリの新しい標準を追加する

ここで行う編集は 1 つです。コードには TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを追加するという、プロジェクト全体のルールを設定します。後半の演習で issue を割り当てると、cloud agent はこのルールを参照します。

編集前に、作業用ブランチを作成します（cloud agent は issue の対象ブランチからインストラクションを読みますが、ここでは編集をきちんとコミットします）。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コード生成の品質を高めるため、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパー関数にはドキュメントコメントがありません。
3. 既存の標準のすぐ下に、次の Markdown を追加します。ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、インストラクションファイルはリポジトリレベル（`.github/copilot-instructions.md`）に置いて全体に適用することも、`*.instructions.md` として特定の言語、ファイル形式、タスクに適用することもできます。今回追加したドキュメントコメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## インストラクションの更新をコミット、プッシュ、マージする

Cloud agent は、issue が対象とするブランチのインストラクションファイルを読みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。インストラクションを読み込ませるには、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue に割り当てると、cloud agent はこのインストラクションを参照します。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ブランチを使う手順にしているのは、実際のプロジェクトの進め方に近づけるためです。

## まとめと次のステップ

このプロジェクトで Copilot がインストラクションファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このワークショップの後半で cloud agent が作成するプルリクエストを確認すると、追加した標準が反映されていることが分かります。

次は、[初めての issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用インストラクションファイル][instruction-files]
- [カスタムインストラクションを作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタムインストラクションを改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — インストラクションファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
