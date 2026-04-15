```markdown
[FINISH: merged|pr|kept|discarded]

# ブランチ完了結果

## 検証前チェック
- テスト実行: {command} → pass
- lint実行: {command} → pass
- build実行: {command} → pass (該当する場合)

## 選択したオプション
- merged | pr | kept | discarded
- 選択根拠: {1-2文}

## ベースブランチ
- main | {別ブランチ名}

## アクション実行ログ
### merged の場合
- チェックアウト: `git checkout {base}`
- マージ: `git merge --no-ff {feature}` → 成功
- worktree 後片付け: 実施 / 未実施

### pr の場合
- PR URL: {https://github.com/.../pull/N}
- PR タイトル: {title}
- worktree は保持

### kept の場合
- ブランチは保持、次作業のため未終了
- worktree は保持

### discarded の場合
- ユーザーに `discard` 文字列で二重確認済み: yes
- ブランチ削除: 実施
- worktree 後片付け: 実施

## 判定
- **結果**: merged | pr | kept | discarded
- **根拠**: {1-2文}
```

**先頭タグ規約（必須）**

- 1行目は必ず `[FINISH: merged]` / `[FINISH: pr]` / `[FINISH: kept]` / `[FINISH: discarded]` のいずれか
- 検証コマンドが fail した場合はどのタグも出さず `[FINISH: kept]` として中断する
