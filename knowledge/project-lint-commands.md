# プロジェクトlintコマンド知識

## lintコマンドの発見方法

プロジェクトタイプに応じて、以下の優先順でlintコマンドを特定する。

### Go プロジェクト

`Makefile` に `lint` ターゲットが存在するか確認する。

```bash
grep -n "^lint" Makefile
```

| 確認対象 | コマンド例 |
|----------|-----------|
| Makefile `lint` ターゲット | `make lint` |
| golangci-lint 直接実行 | `golangci-lint run ./...` |
| `.golangci.yml` / `.golangci.yaml` の存在 | 設定ファイルあり → `golangci-lint run ./...` |

### Node.js / Next.js プロジェクト

`package.json` の `scripts.lint` を確認する。

```bash
cat package.json | grep '"lint"'
```

| 確認対象 | コマンド例 |
|----------|-----------|
| npm | `npm run lint` |
| yarn | `yarn lint` |
| pnpm | `pnpm lint` |

### Expo プロジェクト

`package.json` の `scripts.lint` を確認する（Next.js と同様）。
Expo 特有の lint ツール（`expo lint`）が存在する場合はそちらを優先する。

## コマンド未発見時の対応

| 状況 | 対応 |
|------|------|
| Makefile に `lint` なし、golangci 設定なし | `go vet ./...` を代替として実行 |
| package.json に `lint` スクリプトなし | TypeScript プロジェクトの場合 `tsc --noEmit` を代替として実行 |
| 上記いずれも該当しない | lint スキップ可。理由をサマリーに記録する |
