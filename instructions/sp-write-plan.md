**最初に `superpowers:writing-plans` スキルを Skill ツールで呼び出し、その手順に従って進めよ。**

直前フェーズで合意済みの spec (`docs/superpowers/specs/{YYYY-MM-DD}-{feature}-design.md`) を、実装者がゼロコンテキストでも遂行可能な粒度 (2-5分/タスク) のチェック可能な計画に分解する。

**やること:**

1. Skill ツールで `superpowers:writing-plans` を invoke し、スキル内の原則 (プレースホルダ禁止、正確なファイルパス必須、コードブロック完全化) を必ず守る
2. 対象featureのspecファイルを読む (パスは `00-plan.md` 等の前workspaceレポートか Previous Response から特定)
3. 計画を以下の粒度に分解:
   - 1タスク = 2-5分で完了できる単位
   - 各タスクに (a) 対象ファイルパス、(b) 実行するコマンドまたは追加するコード、(c) 検証方法 を含める
   - TBD / TODO / プレースホルダ `{...}` を残したタスクは不可
4. タスク間の依存と並列可否を明記する
5. 生成結果を `docs/superpowers/plans/{YYYY-MM-DD}-{feature}.md` に保存
6. `git add` + コミット `docs(plan): add implementation plan for {feature}`
7. 出力契約 `sp-plan-created` に従い結果をレポートする

**重要:**

- タスク粒度が大きすぎる (1タスク10分超) と判定した場合は、さらに分割する
- specから導けない曖昧点は仮決めせず `[PLAN: needs-revision]` を返して brainstorm に戻す

**判定:**

- 全タスクが粒度ルールを満たし、具体ファイル・コマンドがある → `[PLAN: ready]`
- 欠けたタスクまたは曖昧箇所あり → `[PLAN: needs-revision]`
