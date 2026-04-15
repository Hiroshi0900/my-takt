```markdown
[SPEC: ready|needs-revision]

# 仕様: {feature名}

## 仕様ファイル
- path: `docs/superpowers/specs/{YYYY-MM-DD}-{feature}-design.md`
- commit: {SHA}

## 合意された問題
{1-2文で問題ステートメント}

## 採用アプローチ
{1-2文で採用された設計アプローチ}

## 代替案 (検討し却下)
- {案1}: {却下理由}
- {案2}: {却下理由}

## スコープ境界
### In Scope
- {項目1}
- {項目2}

### Out of Scope
- {項目1}
- {項目2}

## 未解決事項
- {項目} or なし

## 判定
- **結果**: ready | needs-revision
- **根拠**: {1-2文}
```

**先頭タグ規約（必須）**

- 1行目は必ず `[SPEC: ready]` または `[SPEC: needs-revision]` のみ
- タグ行の前後に空行・コメント・他のテキストを置かない
- タグを欠いたレポートは無効として扱う
