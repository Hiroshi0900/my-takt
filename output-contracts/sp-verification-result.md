```markdown
[VERIFY: pass|fail]

# 検証結果

## 実行したコマンド
- {command1}: {pass|fail}
- {command2}: {pass|fail}

## 実行出力の要約
```
{最終行またはエラー要旨を30行以内で引用}
```

## カバレッジ確認
- 計画ファイルの各タスクに対応するテスト: {すべて存在|欠落あり}
- 欠落がある場合の一覧: {T03, ...} or なし

## 判定
- **結果**: pass | fail
- **根拠**: {1-2文。必ず実行したコマンドの出力に基づく、"should"/"probably" 禁止}
```

**先頭タグ規約（必須）**

- 1行目は必ず `[VERIFY: pass]` または `[VERIFY: fail]` のみ
- 実行コマンドの出力を読まずに `pass` を宣言してはならない
- 未実行・未読は `fail` として扱う（superpowers:verification-before-completion の鉄則）
