```markdown
# PR準備: {feature名}

## PR Title
{conventional commit形式のタイトル、70文字以内}

## PR Body

### Summary
{変更の目的と概要、3-5文}

### Changes
- {主要な変更点1}
- {主要な変更点2}

### SDD Report
> {summary.md の内容を引用}

### Test Plan
- {実施済みテスト1}
- {実施済みテスト2}

### Breaking Changes
{破壊的変更の内容、またはなし}

## 変更統計
- ファイル数: {N}
- 追加行: {+N}
- 削除行: {-N}

## コミット履歴
- {commit message 1}
- {commit message 2}

## セルフレビュー結果

### 検出された問題
| Severity | 内容 | ファイル | 行 |
|----------|------|---------|-----|
| {Critical/Warning} | {指摘内容} | {ファイルパス} | {行番号} |

{問題なしの場合: 「問題は検出されませんでした」}

### 判定
- **結果**: 準備完了 / 重大な問題あり / 準備不可
- **根拠**: {1-2文}
```
