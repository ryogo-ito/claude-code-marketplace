# ryogo-ito's Claude Code Marketplace

Claude Code 用のプラグインマーケットプレイス。開発ワークフローを効率化するスキルを提供。

## インストール

### マーケットプレイスの追加

```bash
/plugin marketplace add ryogo-ito/claude-code-marketplace
```

### プラグインのインストール

```bash
/plugin install ryogo-ito@ryogo-ito-claude-code-marketplace
```

## 利用可能なスキル

| スキル | コマンド | 説明 |
|--------|----------|------|
| task | `/task` | 要件からPR作成までの一連の実装プロセスを実行 |
| parallel-task | `/parallel-task` | git worktreeを使って複数タスクを並列実行 |
| worktree | `/worktree <branch>` | git worktree環境をセットアップ |
| worktree-cleanup | `/worktree-cleanup` | マージ済みPRのworktreeをクリーンアップ |

## スキル詳細

### /task

要件を受け取り、調査・計画・実装・PR作成までを一貫して行うワークフロースキル。

**フェーズ:**
1. 要件の収集
2. 影響範囲の調査
3. 実装計画の策定
4. エッジケース・実装仕様の検討
5. 計画の提出と承認
6. 実装
7. コミットの分割と作成
8. PR作成

### /parallel-task

複数の実装タスクをgit worktreeで並列実行。

**フェーズ:**
1. タスクの収集（複数タスクを入力）
2. Worktreeの作成（各タスクごと）
3. 並列実行（各worktreeで/taskを実行）
4. 完了待機と結果確認
5. Push前の確認
6. Push & PR作成

### /worktree

指定したブランチ名でgit worktree環境を作成。依存関係のインストールと環境変数のコピーも実行。

```bash
/worktree feat/new-feature
```

### /worktree-cleanup

マージ済みPRに対応するworktreeを検出して削除。

```bash
/worktree-cleanup
```

## ライセンス

MIT
