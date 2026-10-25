---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッション共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同じように、GitHub Copilot CLI にも多くのスラッシュコマンドがあります。これらのコマンドで、高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、他にも役立つコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話コンテキストをどう管理するかを確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

Copilot CLI の基本的なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への作業の委任など、追加機能を確認しましょう。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システムインストラクション、ツール、メッセージ、空き領域の内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が短い場合、変化がほとんどないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを自分で決めたい場合は `/compact` を使います。関係のない別のタスクを始める場合は、代わりに `/clear` または `/new` を使って新しい会話を開始します。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、モデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法をチーム全体で改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown エクスポートに使う `/share file` など、目的に合った方法を選択します。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 並列化または委任する

ワークショップの基本部分は完了です。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割して、並列で実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送信できます。クラウドエージェントは非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドは、追加の worktree やリモート作業を伴う場合があるため、任意の機能です。試すときは、範囲を明確にした新しいタスクを始め、通常のワークフローで結果をレビューしてください。非同期エージェントの詳細については、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話コンテキストをどう管理するかを確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認しました。

Copilot CLI には、さらに多くのスラッシュコマンドがあります。[これまでの学習内容と次のステップを振り返り][next-lesson]、学習を続けましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
