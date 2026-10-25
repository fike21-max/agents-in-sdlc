---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使って、コード生成のガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の指示を適用した状態で、フィルタリング機能の最初のデータ スライス（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリング用のデータ スライスを `main` に commit し、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot の出力は確定的ではありません。この演習では、指示を更新した後の出力がどのように変化するかを確認します。出力が文字単位で一致することを目指すものではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発作業に関するガイドラインと要件があります。

- データ層には必ず unit test を用意します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc の doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、これらのプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、Copilot にコンテキストや好みを伝え、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、作成中のものの概要、ベスト プラクティス、Copilot Chat または CLI へのほとんどのリクエストに関係するプロジェクト情報など、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に合わせて作成できます。TypeScript などの言語や、UI コンポーネントや unit test の作成などのタスクに関するガイダンスを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` front matter を持つ `*.instructions.md` ファイルの両方を使います。
>
>
> GitHub Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準形式の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプル プロジェクトには一般的な方法が示されています。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、作成中のもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、unit test、Astro コンポーネント、データ層など、ファイル種別やタスクに固有の指示を記載します。
- 自然な言葉で明確に指示し、コードの良い例と悪い例を示します。

指示ファイルの作り方や AI の使い方に、唯一の正解はありません。プロジェクトに適した方法を試しながら見つけていきましょう。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルのセットを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。各環境で操作方法が異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init`）。関連する場面で、このレッスン内で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読みましょう。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle のデータ層、test などの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認してください。この値はリポジトリ ルートからの glob で、指示を適用するファイルを指定します。たとえば、`**/*.test.ts` に一致する TypeScript test ファイルが対象になります。
5. このプロジェクトで unit test を作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル先頭のコメントを追加するルールを設定します。

## 変更用のブランチを作成する

ここでは、ベスト プラクティスに従って変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に示します。
3. Chat ビューの agent ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。helper ファイルとその test など、複数のファイルが変更されることがあります。
6. 提案された helper が、最初の引数として `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的なため、指示がなくても doc comment が追加される場合があります。その場合も問題ありません。指示を更新した後に一貫性が向上することを確認するのがこの演習の目的です。

## リポジトリの標準を追加する

前述のとおり、`.github/copilot-instructions.md` には Copilot に伝えるプロジェクト レベルの情報を記載します。コードの提案を改善するため、コーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化に関するルールはありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準の直下に、次の Markdown を追加します。ファイル先頭のコメントと TSDoc の doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` に全体的な指示を記載できます。また、言語、ファイル種別、タスクに特化した `*.instructions.md` ファイルも作成できます。今回追加した doc comment のルールのようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc の doc comment が追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで Copilot に新しいプロジェクト標準を伝え、次の演習で基盤として使う実際のコードに適用できました。

## 指示とブランチを commit して push する

指示ファイルもリポジトリ内の他のファイルと同様、ソース管理で管理します。指示の更新とブランチを commit して、リポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と helper の基盤に関して Copilot が変更したファイルをすべて stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成して merge する

ブランチを push したら、バックログにあるドキュメント標準の issue に関連付けた pull request を作成します。手作業でも作成できますが、GitHub のツールに接続済みの Copilot に依頼して、issue を見つけ、issue を解決する PR を作成し、その後 merge します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub のツールがあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。次の演習で、この接続に使われている Model Context Protocol について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と、新しい publishers helper の基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 上で操作することを許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
5. Copilot が issue を見つけて PR を作成する作業を始めます。
6. PR が作成されたら、Copilot に PR を merge して、ローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 上で操作したり、shell command を実行したりすることを許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。

これで GitHub Copilot の支援を受けて pull request を作成し、merge できました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を適用した状態でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新と helper の基盤を `main` に commit し、次の演習で利用できるようにしました。

次は [agent mode][next-lesson] で指示を活用し、Copilot にコードベース全体へ新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を作成する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
