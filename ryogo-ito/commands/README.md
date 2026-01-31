# スキル一覧

このディレクトリには以下のスキルが含まれています。

## /task

要件からPR作成までの一連の実装プロセスを実行するワークフロースキル。

### 使い方

```bash
/task
```

### ワークフロー

| Phase | 内容 |
|-------|------|
| 1 | 要件の収集（口頭説明またはドキュメントリンク） |
| 2 | 影響範囲の調査（変更が必要なファイル・関数の特定） |
| 3 | 実装計画の策定（具体的な実装手順の洗い出し） |
| 4 | エッジケース・実装仕様の検討 |
| 5 | 計画の提出と承認（ユーザー承認後に次へ進む） |
| 6 | 実装（ブランチ作成、コード実装、型チェック・リント） |
| 7 | コミットの分割と作成（Conventional Commits形式） |
| 8 | PR作成（push & `gh pr create`） |

### 特徴

- 各Phaseで不明点があれば先に進む前にユーザーに確認
- Phase 5の承認なしに実装を開始しない
- 専門エージェント（Explore, Plan, cook:roles:*）を活用

---

## /parallel-task

git worktreeと/taskスキルを組み合わせて複数タスクを並列実行。

### 使い方

```bash
/parallel-task
```

起動後、実行したいタスクを複数入力。

### ワークフロー

| Phase | 内容 |
|-------|------|
| 1 | タスクの収集（複数タスクとベースブランチを入力） |
| 2 | Worktreeの作成（各タスクごとに環境構築） |
| 3 | 並列実行（各worktreeで/task Phase 1-7を実行） |
| 4 | 完了待機と結果確認 |
| 5 | Push前の確認（個別に承認/却下可能） |
| 6 | Push & PR作成（承認されたタスクのみ） |

### 特徴

- 各タスクは独立したworktreeで実行され、互いに影響しない
- 失敗したタスクがあっても、成功したタスクは続行可能
- 完了後、不要なworktreeは `/worktree-cleanup` で削除可能

---

## /worktree

指定したブランチ名でgit worktree環境を作成。

### 使い方

```bash
/worktree feat/new-feature
```

### 実行内容

1. **Worktree作成**: `git worktree add -b <ブランチ名> ../<ディレクトリ名>`
2. **依存関係のインストール**: package-lock.json, pnpm-lock.yaml, yarn.lock, bun.lockb を検出して適切なパッケージマネージャを実行
3. **環境変数ファイルのコピー**: .env, .env.local, .env.development など

### ディレクトリ命名規則

ブランチ名の `/` を `-` に置換:
- `feat/add-auth` → `<プロジェクト名>-feat-add-auth`

---

## /worktree-cleanup

マージ済みPRに対応するworktreeを検出して削除。

### 使い方

```bash
/worktree-cleanup
```

### 実行内容

1. 既存のworktreeを一覧表示
2. 各worktreeのブランチに対応するPRがマージ済みかチェック
3. マージ済みのworktreeを削除（確認あり）
