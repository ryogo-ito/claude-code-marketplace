---
description: Create a git worktree for parallel development with Claude
allowed-tools: Bash, Read, Glob
---

# Git Worktree セットアップ

ブランチ名: $ARGUMENTS

以下の手順で並行開発環境を構築してください:

## 1. Git Worktree の作成

ディレクトリ名はブランチ名の `/` を `-` に置換したものを使用:
- 例: `feat/test-hoge` → `feat-test-hoge`

```bash
# ブランチ名の / を - に置換してディレクトリ名を生成
dir_name=$(echo "<ブランチ名>" | tr '/' '-')
git worktree add -b <ブランチ名> ../<現在のディレクトリ名>-${dir_name}
```

## 2. 環境構築

新しく作成したworktreeディレクトリで:

### 依存関係のインストール
- package-lock.json があれば `npm install`
- pnpm-lock.yaml があれば `pnpm install`
- yarn.lock があれば `yarn install`
- bun.lockb があれば `bun install`

### 環境変数ファイルのコピー
元のプロジェクトから以下のファイルをコピー:
- .env
- .env.local
- .env.development
- .env.development.local
- .env.production.local
- その他 .env* にマッチするファイル

## 3. 完了報告

セットアップ完了後、作成したworktreeのパスと次のステップを報告してください。
