# Instruction: implement-generic

## 目的

設計レポートに基づいてGoコードを実装する。プロジェクトの DDD / Clean Architecture パターンに準拠し、ビルド可能な実装を提供する。

## 手順

### Step 1: 実装順序の決定

依存関係の順番で実装する：

```
1. ドメイン型（値オブジェクト・エンティティ）
2. リポジトリインターフェース
3. ユースケースI/F + Input/Output
4. ユースケース実装
5. DTO変換（converter/mapper）
6. エラーマッピング
7. DI wiring（main.go / wire.go / fx/google wire 等）
```

### Step 2: 値オブジェクト実装

- 型定義
- 検証ルール（Specification/Validator）
- 安全コンストラクタ（SafeNew*）
- 必要なら内部用コンストラクタ（New*）
- `Validate()` メソッド（または同等API）

### Step 3: エンティティ/集約実装

- インターフェース定義（必要なら）
- 状態型（必要なら）
- getter / コマンドメソッド
- 状態遷移ルールをメソッドに封じ込める

### Step 4: ユースケース実装

```go
type xxxImpl struct { repo /* deps */ }

func NewXxxUseCase(/* deps */) usecase.XxxUseCase {
    return &xxxImpl{/* deps */}
}

func (u *xxxImpl) Execute(ctx context.Context, input usecase.XxxInput) (*usecase.XxxOutput, error) {
    // 1. 入力検証（SafeNew* 等）
    // 2. リポジトリ取得
    // 3. ドメインコマンド実行
    // 4. 保存
    // 5. Outputに詰めて返す
}
```

### Step 5: エラーマッピング

- 新規エラー型を追加した場合、境界変換（HTTP/gRPC等）にマッピングを追加する

### Step 6: DI wiring

- composition root に `Provide` / `Bind` / `wire` 設定を追加する

## 実装後チェック（必須実行）

以下を順番に実行し、すべて成功してから完了とする。

### lintゲート（必須）

プロジェクトのlintコマンドを以下の優先順で特定して実行する:
- Go: `Makefile` に `lint` ターゲットがあれば `make lint`、なければ `golangci-lint run ./...`
- Node.js / Next.js / Expo: `package.json` の `scripts.lint` があれば `npm run lint`（または yarn/pnpm）
- 上記に該当しない場合: `go vet ./...` または `tsc --noEmit` を代替として実行

lintが失敗した場合はエラーを修正してから再実行する。コマンドが見つからない場合はスキップ可（理由を記録する）。

### その他チェック

```
[ ] build が通る（例: go build ./...）
[ ] domain層に禁止importがない
[ ] 新規公開型に対応するテストがある
[ ] converter/mapper が更新されている（必要な場合）
[ ] composition root の wiring が更新されている
```
