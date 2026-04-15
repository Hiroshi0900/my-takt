```markdown
[IMPL: complete|partial|blocked]

# 実装状況: バッチ {N}

## 対象タスク
- {T01, T02, ...}

## 変更ファイル
| 種別 | ファイル | 行数 |
|------|---------|------|
| 作成 | `path` | +NN |
| 変更 | `path` | ±NN |

## TDDサイクル
| タスク | RED | GREEN | REFACTOR |
|--------|-----|-------|----------|
| T01 | ✅ 失敗テスト追加 | ✅ 通過 | {適用した改善 or なし} |

## 完了タスク
- {T01, T02, ...}

## 未完了タスク
- {ID}: {理由}

## 検証
- lint: {command} → {pass|fail|未実行}
- test: {command} → {pass|fail|未実行}
- build: {command} → {pass|fail|未実行}

## 次のアクション
- {次バッチ計画 or verify へ進む or blocker 解除が必要}

## 判定
- **結果**: complete | partial | blocked
- **根拠**: {1-2文}
```

**制約（必須・時間ガード）**

- このインストラクション1回の実行で触るファイル数は **最大3**、所要時間は **最大10分**
- 上限に達したら即座に `partial` として完了させ、次バッチに持ち越す
- マラソンセッション禁止

**先頭タグ規約（必須）**

- 1行目は必ず `[IMPL: complete]` / `[IMPL: partial]` / `[IMPL: blocked]` のいずれか
- `complete`: バッチ内全タスクがTDDサイクル完走＋lint/test合格
- `partial`: 時間/ファイル上限到達で打ち切り、次バッチで継続可能
- `blocked`: 外部要因・情報不足で継続不可、supervise へエスカレート
