---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング機能、インストラクション、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング機能、インストラクションの更新、スキルの更新、品質保証（QA）プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト（PR）は自分でマージしましたが、今度は**Agent Merge**にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、インストラクションの更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは手順を簡略化するため、フィルタリング機能と品質ワークフロー全体を 1 つの会話とブランチにまとめ、1 つの PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、開発者がマージの許可を管理したまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot は失敗した継続的インテグレーション（CI）チェックへの対応、レビューコメントへの回答、必要に応じたリベースなど、PR をブロックしている問題に対処します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードがすべて作成できたので、まとめてレビューし、PR を作成して、Agent Merge に残りの作業を任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリが許可する場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証に続き、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える[便利な Copilot CLI のスラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
