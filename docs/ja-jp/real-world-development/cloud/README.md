---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)**を使うと、GitHub Copilot をクラウドで非同期に動作させられます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を加え、プルリクエストを作成します。その間、ほかの作業を続けられます。

この演習では、まず cloud agent が使うカスタムインストラクションを追加し、GitHub issue を割り当てて実装を任せます。カスタムエージェントを確認してその進め方を調整し、エージェントのダッシュボードからセッションを監視・誘導して、最後にプルリクエストをレビューし、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタムインストラクション][ex1] | コンテキスト | cloud agent が使うカスタムインストラクションを追加する |
| [2. Cloud Agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して誘導する |
| [5. 反復的な改善][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備ができていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用できませんか？認定を受けた学生は、[GitHub Education][callout-student-plan-education]を通じて無料で GitHub Copilot を利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべての演習を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise の場合、管理者が組織で有効にする必要があります。

> [!NOTE]
> このワークショップでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ワークショップ](../cli/)または [VS Code ワークショップ](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
