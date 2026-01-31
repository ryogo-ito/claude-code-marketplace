---
name: parallel-task
description: git worktreeと/taskスキルを組み合わせて複数タスクを並列実行するスキル。「/parallel-task」で起動し、複数の実装タスクを同時に進めたいときに使用する。各タスクは独立したworktreeで実行され、完了後にまとめてPRを作成する。
---

# Parallel Task

複数の実装タスクをgit worktreeを使って並列実行するワークフロースキル。

## 起動条件

- ユーザーが `/parallel-task` を実行したとき

## ワークフロー

### Phase 1: タスクの収集

1. ユーザーに以下を質問する:
   - **実行したいタスク**: 複数のタスクをリスト形式で入力してもらう
   - **ベースブランチ**: どのブランチから切るか（デフォルト: 現在のブランチ）

2. 各タスクに対して適切なブランチ名を提案する
   - 例: `feat/add-auth`, `fix/login-bug` など
   - ユーザーの承認を得る

### Phase 2: Worktree の作成

各タスクに対して以下を実行:

1. ブランチ名から worktree ディレクトリ名を生成
   - `/` を `-` に置換
   - 例: `feat/add-auth` → `<現在のディレクトリ名>-feat-add-auth`

2. worktree を作成
   ```bash
   dir_name=$(echo "<ブランチ名>" | tr '/' '-')
   git worktree add -b <ブランチ名> ../<現在のディレクトリ名>-${dir_name}
   ```

3. 環境構築
   - 依存関係のインストール（package-lock.json → npm, pnpm-lock.yaml → pnpm, etc.）
   - 環境変数ファイルのコピー（.env, .env.local, .env.development など）

### Phase 3: 並列実行

各 worktree で Task ツールを使用してサブエージェントを **並列で** 起動する。

- `subagent_type`: `general-purpose`
- `run_in_background`: `true`

各サブエージェントへの指示:

```
作業ディレクトリ: /path/to/worktree

以下のタスクを実装してください:
[タスクの要件]

/task スキルの Phase 1-7 に従って実装してください。

重要:
- ブランチは既に作成済みです。Phase 6 のブランチ作成はスキップしてください。
- Phase 7（コミット）まで完了したら停止してください。
- Phase 8（push & PR作成）は実行しないでください。
```

### Phase 4: 完了待機と結果確認

1. 全てのサブエージェントの完了を待つ
   - TaskOutput ツールで各タスクの進捗を確認

2. 各 worktree の結果をまとめて報告
   - 成功したタスク
   - 失敗したタスク（あれば原因も）
   - 各タスクの変更サマリ（`git log --oneline`, `git diff <base-branch>...HEAD --stat`）

### Phase 5: Push 前の確認

1. 全タスクの結果をユーザーに提示

2. ユーザーに確認を求める
   - 「これらの変更を push して PR を作成しますか？」
   - 個別に承認/却下できるようにする

### Phase 6: Push & PR 作成

承認されたタスクに対して、サブエージェントを再度起動し Phase 8 を実行させる:

```
作業ディレクトリ: /path/to/worktree

/task スキルの Phase 8 を実行してください。
- リモートに push
- PR を作成

ベースブランチ: <base-branch>
```

作成された PR の URL をまとめて報告する。

## 注意事項

- 各タスクは独立した worktree で実行されるため、互いに影響しない
- サブエージェントは Phase 7 まで実行し、push は行わない
- 失敗したタスクがあっても、成功したタスクは続行可能
- 完了後、不要な worktree は `/worktree-cleanup` で削除可能
