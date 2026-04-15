**アーキテクチャと設計**のレビューに集中してください。
AI特有の問題はレビューしないでください（ai_reviewムーブメントで実施済み）。

## スキル参照（Read で読み込み・レビュー前に必須）

以下のスキルファイルを Read で読み込み、チェック観点として使用してください:

- `~/.claude/skills/clean-architecture/SKILL.md` — 層構造違反、依存方向違反の検出パターン
- `~/.claude/skills/repository-placement/SKILL.md` — リポジトリインターフェースの配置ルール
- `~/.claude/skills/tell-dont-ask/SKILL.md` — 状態問い合わせ→外部判断パターンの検出
- `~/.claude/skills/law-of-demeter/SKILL.md` — Train Wreck（連鎖呼び出し）の検出

**DDD プロジェクトの場合（ナレッジが DDD/Clean Architecture パターンを含む場合）:**
- `~/.claude/skills/aggregate-design/SKILL.md`
- `~/.claude/skills/domain-building-blocks/SKILL.md`
- `~/.claude/skills/repository-design/SKILL.md`

## レビュー観点

### 構造・設計
- 構造・設計の妥当性
- モジュール化（高凝集・低結合・循環依存）
- 関数化（1関数1責務・操作の一覧性・抽象度の一致）
- コード品質（上記スキルの基準を適用）
- 変更スコープの適切性
- テストカバレッジ
- デッドコード
- 呼び出しチェーン検証
- 契約文字列（ファイル名・設定キー名）のハードコード散在

### Go 並行処理（Go プロジェクトの場合）

```
[ ] goroutine がリークしていないか（defer cancel()、WaitGroup、done チャネル）
[ ] context が正しく伝播しているか（context.Background() の安易な使用を検出）
[ ] mutex と channel の使い分けが適切か（共有状態には mutex、所有権移転には channel）
[ ] race condition の可能性があるか（複数 goroutine が同じ変数に書き込む箇所）
[ ] select 文の default 分岐がスピンループを生んでいないか
[ ] sync.Once / sync.Map の使用が適切か
```

### パフォーマンス

```
[ ] N+1 クエリが発生していないか（ループ内でのDB呼び出しを検出）
[ ] リソースリークがないか（file, connection, ticker を Close/Stop しているか）
[ ] コネクションプールが適切に管理されているか
[ ] 大量データを一括ロードしていないか（ページネーションが必要な箇所）
[ ] 不要なメモリアロケーションがないか（ループ内の大きなスライス作成等）
```

## 設計判断の参照

{report:coder-decisions.md} を確認し、記録された設計判断を把握してください。
- 記録された意図的な判断は FP として指摘しない
- ただし設計判断自体の妥当性も評価し、問題がある場合は指摘する

## 前回指摘の追跡（必須）

- まず「Previous Response」から前回の open findings を抽出する
- 各 finding に `finding_id` を付け、今回の状態を `new / persists / resolved` で判定する
- `persists` と判定する場合は、未解決である根拠（ファイル/行）を必ず示す

## 判定手順

1. 参照スキルファイルを Read で読み込む
2. 前回 open findings を抽出し、`new / persists / resolved` を仮判定する
3. 変更差分を確認し、構造・設計・並行処理・パフォーマンスの観点に基づいて問題を検出する
   - ナレッジの判定基準テーブル（REJECT条件）と変更内容を照合する
4. 検出した問題ごとに、Policyのスコープ判定表と判定ルールに基づいてブロッキング/非ブロッキングを分類する
5. ブロッキング問題（`new` または `persists`）が1件でもあればREJECTと判定する
