作業ブランチが `main` に対して古すぎないかを確認し、結果をレポートせよ。

**目的:**
- 実装前に `main` との差分状況を把握する
- 古いベース由来のテスト失敗・ビルド失敗・競合リスクを下げる
- 自動で merge/rebase は行わず、同期が必要なら推奨を出す

**やること:**
1. Git リポジトリか確認する（`git rev-parse --is-inside-work-tree`）
2. 現在ブランチを取得する
3. `main`（なければ `origin/main`）の存在を確認する
4. 取得可能な場合、以下を確認する
   - `HEAD` のコミット
   - `main` のコミット
   - `merge-base HEAD main`
   - ahead/behind 件数（`git rev-list --left-right --count HEAD...main`）
5. 判定する
   - `同期不要（継続推奨）`: `behind=0`
   - `同期推奨（継続可）`: `behind>0`
   - `確認不可（継続可）`: git repo でない / `main` がない / コマンド失敗
6. `make test` / `make build` 実行前に同期した方がよいかを短く助言する

**重要:**
- `git merge`, `git rebase`, `git pull` は実行しない
- コード編集はしない
- 取得した事実だけをレポートする

**必須出力**

```markdown
# ブランチ最新化チェック

## 対象
- branch: {現在ブランチ or 不明}
- baseline: {main / origin/main / 不明}

## 判定
- status: 同期不要（継続推奨） / 同期推奨（継続可） / 確認不可（継続可）

## Git状態
- head: {sha or 不明}
- baseline_head: {sha or 不明}
- merge_base: {sha or 不明}
- ahead: {数 or 不明}
- behind: {数 or 不明}

## 推奨アクション
- {例: 実装前に main を取り込んで `make test` / `make build` を実行してから再開する}
```
