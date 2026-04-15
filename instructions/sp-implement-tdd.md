**最初に `superpowers:subagent-driven-development` と `superpowers:test-driven-development` の両方を Skill ツールで invoke し、両スキルの原則を遵守せよ。**

計画ファイル (`docs/superpowers/plans/{YYYY-MM-DD}-{feature}.md`) から次に実装すべきタスクを特定し、TDDサイクル (RED → GREEN → REFACTOR) で実装する。

**時間/範囲ガード（マラソン防止・最優先）:**

- 本インストラクション1回の実行で触るファイルは **最大3ファイル**
- 経過時間 **10分** 到達時点で即座に `[IMPL: partial]` を返して次バッチへ
- 1つのサブエージェントで扱うのは **1タスク** まで (複数まとめて実装しない)
- 連続 Edit が30回を超えたら即座に打ち切る

**やること:**

1. `docs/superpowers/plans/` から最新計画ファイルを特定 (`ls -t` で最新)
2. 計画ファイル内で未チェック (`- [ ]`) の先頭タスクを1-3件ピック (バッチ)
3. 各タスクについて TDD サイクル:
   - **RED**: 期待挙動のテストを先に書き、`make test` or プロジェクトのテストコマンドで失敗を確認
   - **GREEN**: テストが通る最小実装のみ書く (過剰実装禁止)
   - **REFACTOR**: 重複・命名を整える (外部挙動は不変)
4. `make lint` と `make test` を実行、両方の成功を確認
5. 計画ファイルの該当タスクを `- [x]` に更新
6. 変更をステージして `git commit -m "{type}({scope}): {what}"` (Conventional Commits)
7. 出力契約 `sp-implementation-status` に従いレポート

**TDD 鉄則（superpowers より継承）:**

- テストより先にプロダクションコードを書かない
- テストがない機能を "complete" と判定しない
- 失敗するテストを見ずに次へ進まない

**判定:**

- バッチ内全タスクがTDDサイクル完走、lint/test合格 → `[IMPL: complete]`
- 時間/ファイル上限で打ち切り、残タスクあり → `[IMPL: partial]`
- 仕様の曖昧性・環境起因等で継続不可 → `[IMPL: blocked]`
