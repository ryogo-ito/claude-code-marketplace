# ryogo-ito's Claude Code Marketplace

Claude Code 用のプラグインマーケットプレイス。開発ワークフローを効率化するスキルを提供。

## インストール

### 1. マーケットプレイスの追加

```bash
/plugin marketplace add ryogo-ito/claude-code-marketplace
```

### 2. プラグインのインストール

```bash
/plugin install ryogo-ito@ryogo-ito-claude-code-marketplace
```

### インストール確認

```bash
/plugin list
```

## 利用可能なスキル

| スキル | コマンド | 説明 |
|--------|----------|------|
| task | `/task` | 要件からPR作成までの一連の実装プロセスを実行 |
| parallel-task | `/parallel-task` | git worktreeを使って複数タスクを並列実行 |
| worktree | `/worktree <branch>` | git worktree環境をセットアップ |
| worktree-cleanup | `/worktree-cleanup` | マージ済みPRのworktreeをクリーンアップ |

各スキルの詳細は [ryogo-ito/commands/README.md](./ryogo-ito/commands/README.md) を参照。

## プラグイン管理

```bash
# マーケットプレイス一覧
/plugin marketplace list

# マーケットプレイス更新
/plugin marketplace update ryogo-ito-claude-code-marketplace

# プラグイン無効化
/plugin disable ryogo-ito@ryogo-ito-claude-code-marketplace

# プラグイン有効化
/plugin enable ryogo-ito@ryogo-ito-claude-code-marketplace

# アンインストール
/plugin uninstall ryogo-ito@ryogo-ito-claude-code-marketplace

# マーケットプレイス削除
/plugin marketplace remove ryogo-ito-claude-code-marketplace
```

## ライセンス

MIT
