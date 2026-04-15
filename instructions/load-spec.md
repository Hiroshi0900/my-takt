既存の superpowers 形式の spec ファイル (brainstorm 成果物) を読み込み、write-plan に進められる状態かを確認する。

**やること:**

1. `docs/superpowers/specs/` 配下の `.md` ファイルを `ls -t` で列挙
2. ユーザータスク指示に feature 名がある場合は `{YYYY-MM-DD}-{feature}-design.md` の形式で絞り込む
3. 最新の spec ファイルを1つ特定
4. spec ファイルを読み、以下を確認:
   - `[SPEC: ready]` タグが先頭にあるか
   - 「採用アプローチ」「スコープ境界」「未解決事項」セクションが揃っているか
   - プレースホルダ `{...}` が残っていないか

**必須出力:**

```markdown
# Specロード結果

## Specファイル
- path: `docs/superpowers/specs/{file}`
- ready タグ: あり | なし
- 未解決事項: なし | あり ({概要})

## 判定
- **結果**: Spec有効 / Spec不足・不備
- **根拠**: {1-2文}
```

**判定ルール:**

- `[SPEC: ready]` タグあり、主要セクション存在、プレースホルダなし、未解決事項なし → `Spec有効`
- いずれか欠ける → `Spec不足・不備` (呼び出し元で ABORT する)
