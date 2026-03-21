# リファクタ分析レポート: {対象}

## サマリー
- {対象範囲}
- {主要な構造問題}
- {推奨結論}

## 観測した構造問題
| ID | 種類 | 重大度 | 根拠 | 説明 |
|----|------|--------|------|------|
| R-01 | {循環依存 / 神モジュール / 散在 / 技術駆動パッケージ / 安全性不足} | {High/Medium/Low} | `{path}` | {説明} |

## 共通コア化候補
- `{movement / instruction / output-contract}`: {どこで重複しているか}
- `{movement / instruction / output-contract}`: {どの粒度で共通化できるか}

## 適合性判定

### `refactor-packages`
- 判定: `{strong-fit / fit / weak-fit / no}`
- 理由:
  - {理由}

### `refactor-domain`
- 判定: `{strong-fit / fit / weak-fit / no}`
- 理由:
  - {理由}

### `refactor-safe`
- 判定: `{strong-fit / fit / weak-fit / no}`
- 理由:
  - {理由}

## 推奨
- **第一候補**: `{refactor-packages / refactor-domain / refactor-safe / none}`
- **理由**: {理由}
- **推奨順**: {必要なら 1 → 2}

## 非推奨または保留
- `{候補}`: {理由}

## PR分割方針
- {小さな変更単位}
- {レビューしやすい粒度}

## 実行前提
- {不足しているテスト}
- {不足している設計情報}
- {不足している境界整理}
