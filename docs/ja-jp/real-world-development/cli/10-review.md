---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI の workflow、2 つの pull request、再利用可能なカスタマイズ、今後のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の workflow に取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルタリング issue を見つけました。
- star rating を追加し、forward した browser で結果を確認して、最初の pull request (PR) を手動で merge しました。
- フィルタリング issue から開始し、Plan mode で方針を決め、Autopilot mode で実装し、Interactive mode でレビューしました。
- カスタム指示で agent をガイドし、既存の `quality-checks` skill をカスタマイズして、unit test、lint、type check を実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際の browser でフィルタリング機能を確認しました。
- quality assurance (QA) カスタム エージェントを作成して選択し、要件、coverage、skill の結果、browser の証拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に対する Agent Merge を承認しました。
- context、model、共有、任意の cloud delegation に関する slash command を確認しました。

## 公開した内容

このワークショップには、更新された `main` からそれぞれ別のブランチで作成する 2 つの PR の節目があります。

1. **star rating:** ゲーム カードに既存の `starRating` と、評価が未設定であることを明示する状態を表示します。
2. **フィルタリングと品質 workflow:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` の report をカスタマイズし、QA profile を作成し、関連する test を追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。作業を効率化するため、これらの変更は 1 つの PR にまとめました。

## さまざまな検証方法

自動 test、自分で行う browser の確認、MCP を使った Copilot の browser 調査など、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、type check を実行し、指定した新しい形式で結果を報告しました。QA では、PR の前に要件と test coverage のレビューを結果と合わせて確認しました。

追加した test で実際の不足を補えていれば十分です。新しい test を追加しない QA 実行が適切なこともあります。merge を承認する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与える context と tool によって、作業の結果が変わります。このワークショップでは、指示の更新、skill のカスタマイズ、QA profile の作成、MCP server の設定を行いました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整します。指示は標準を設定し、skills は繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP server は外部 tool に接続します。agent の概要だけでなく、実際の変更と tool の結果を確認してください。

タスクに適した**モードとモデル**を選びます。**Plan** は実装前に方針を検討し、**Interactive** は範囲を絞った変更を進めながら作業に参加し、**Autopilot** は明確に範囲を定めたタスクに使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

環境だけでなく、context も重要です。*何を*作るのか、*なぜ*必要なのか、*どのように*作るのかを明確に伝えると、出力が大きく変わります。

## さらに試す

基本的な workflow を確認しました。Copilot CLI には、ほかにも役立つ機能があります。

- `/review` で code review agent に変更を分析させます。
- `/rubber-duck` で問題について相談し、別の視点を得ます。
- `/fleet` で独立した subtask を並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。production code、趣味のコード、長年作ろうと思いながら手をつけられなかった小さなアプリなどで活用してください。学んだことをチームで共有し、ほかの人からも学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI command reference][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
