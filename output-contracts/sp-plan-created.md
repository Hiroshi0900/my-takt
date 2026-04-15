```markdown
[PLAN: ready|needs-revision]

# 実装計画: {feature名}

## 計画ファイル
- path: `docs/superpowers/plans/{YYYY-MM-DD}-{feature}.md`
- commit: {SHA}
- 参照spec: `docs/superpowers/specs/{YYYY-MM-DD}-{feature}-design.md`

## タスク総数
- 合計: {N}
- 推定合計時間: {M} 分 (各タスク2-5分)

## タスク一覧
| ID | タスク概要 | 想定ファイル | 想定時間 |
|----|-----------|-------------|---------|
| T01 | {1文} | `path/to/file` | 3分 |
| T02 | {1文} | `path/to/file` | 4分 |

## 依存関係
- T02 は T01 完了後
- T03-T05 は並列実行可

## 検証方法
- 各タスク: {テストコマンドやアサーション}
- 全体: {lint / test / build の実行コマンド}

## 判定
- **結果**: ready | needs-revision
- **根拠**: {1-2文。全タスクが2-5分粒度で具体ファイル・コマンド含むかの自己確認}
```

**先頭タグ規約（必須）**

- 1行目は必ず `[PLAN: ready]` または `[PLAN: needs-revision]` のみ
- プレースホルダ `{...}` が残っているタスクは `needs-revision` とする
- タグ行の前後に空行・コメント・他のテキストを置かない
