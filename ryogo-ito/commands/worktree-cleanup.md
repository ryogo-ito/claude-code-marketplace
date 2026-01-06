---
description: Clean up git worktrees whose PRs have been merged
allowed-tools: Bash, Read
---

# Git Worktree クリーンアップ

マージ済みPRのworktreeを自動削除します。

## 手順

### 1. Worktree 一覧の取得

```bash
git worktree list
```

### 2. 各 Worktree のチェックと削除

メインのworktree（bareまたは現在の作業ディレクトリ）以外の各worktreeに対して:

1. ブランチ名を取得
2. `gh pr list --head <ブランチ名> --state merged` でマージ済みか確認
3. マージ済みなら:
   ```bash
   git worktree remove <worktreeパス>
   git branch -d <ブランチ名>  # ローカルブランチも削除
   ```

### 3. Prune（不要な参照の削除）

```bash
git worktree prune
```

## 注意事項

- 削除前に対象のworktreeとブランチ名をユーザーに確認してから実行
- 未コミットの変更がある場合は警告して削除しない
- マージされていないPRのworktreeは残す
