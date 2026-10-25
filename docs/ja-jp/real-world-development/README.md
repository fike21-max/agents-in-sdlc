---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コード作成、動作テスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

あなたは、ボードゲームをテーマにしたクラウドファンディングを提供する架空の会社 Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub の issue として登録済みで、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 開発環境を選択する

GitHub Copilot は、どこで作業していても利用できます。開発方法に合った環境を選択してください。どのワークショップも、それぞれのセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、インストラクション、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、その作業を案内して進捗を監視し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
