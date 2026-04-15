**最初に `superpowers:verification-before-completion` スキルを Skill ツールで invoke し、その鉄則「証拠なしに成功を主張しない」を遵守せよ。**

直前の実装バッチが本当に計画通り完了しているかを、実行コマンドの出力という証拠で確認する。推測・体感・"should"/"probably" 等の語彙は禁止。

**やること:**

1. プロジェクトの検証コマンドを特定 (優先順: `make` / `task` / `just` / package script)
   - 最低限 `lint` と `test`、該当する場合は `build` も
2. 各コマンドを新規に実行 (過去のPrevious Response内の結果を再利用しない)
3. 各コマンドの **最終行** と **エラー/失敗の該当行** を読む (スキップ禁止)
4. 計画ファイル (`docs/superpowers/plans/...`) の全タスクに対応するテストが存在するかを grep で確認
5. 出力契約 `sp-verification-result` に従いレポート

**禁止語彙（検出したら fail とする）:**

- "should pass"
- "probably works"
- "seems to be working"
- "looks good"
- 実行コマンドの引用なしの pass 宣言

**判定:**

- 全検証コマンドが実際に pass、かつ全タスクに対応テスト存在 → `[VERIFY: pass]`
- いずれかの検証失敗、未実行、またはテスト欠落 → `[VERIFY: fail]`
