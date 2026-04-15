```markdown
[DEBUG: resolved|investigating|escalate]

# デバッグ結果

## 対象レビュー指摘
- {指摘ID / ファイル:行 / 概要}

## Phase 1: 調査
- 関連ファイル読了: {リスト}
- 再現テストを追加した: {テストファイル:ケース名} or スキップ（既存テストで再現可）
- 観測された挙動: {1-2文}

## Phase 2: パターン分析
- 既知のアンチパターンと一致: {名前 or なし}
- コードベース内類似箇所: {ファイル:行 or なし}

## Phase 3: 仮説
- 根本原因仮説: {1-2文}
- 反証実験: {やったこと} → {結果}

## Phase 4: 修正
- 修正ファイル: {リスト}
- 修正範囲: 最小差分 ({行数})
- 再現テスト: RED → GREEN 確認

## 試行回数
- 今回通算: {N} 回
- 3回到達で自動エスカレート

## 判定
- **結果**: resolved | investigating | escalate
- **根拠**: {1-2文}
```

**制約（必須）**

- Phase 1 の調査完了前に Phase 4 の修正提案を出してはならない
- 同一指摘での3回目の失敗時は `escalate` を出す（superpowers:systematic-debugging の3回ルール）

**先頭タグ規約（必須）**

- 1行目は必ず `[DEBUG: resolved]` / `[DEBUG: investigating]` / `[DEBUG: escalate]` のいずれか
- `resolved`: 根本原因特定＋最小差分修正＋再現テスト合格
- `investigating`: Phase 1-3 を完了したが Phase 4 未達（情報不足）
- `escalate`: 3回試行で解決せず、上位の設計見直しが必要
