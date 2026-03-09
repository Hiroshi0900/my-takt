# Policy: go-ddd-patterns

## 適用範囲

Goの DDD プロジェクトにおけるドメイン層（例: `internal/{domain}/domain/`）のコード変更に適用する。

## 値オブジェクトパターン

### MUST: 生成時に常に妥当性を保証する

```go
// NewXxx: 信頼されたデータ（内部・DB読み込みなど）- 無効値はpanicでも可
func NewXxx(val string) XxxType {
    v := XxxType(val)
    // Validate() を使う/または同等の検証を行う
    return v
}

// SafeNewXxx: 外部入力（API, UI, メッセージ）- errorを返す
func SafeNewXxx(val string) (XxxType, error) {
    v := XxxType(val)
    if err := XxxSpec.IsSatisfiedBy(v); err != nil {
        return "", err
    }
    return v, nil
}
```

### MUST: ビジネスルールを明示的に表現する

- Specification パターン、Validator、Predicate など、再利用可能な形でルールを表現する
- ルールをハンドラーやユースケースに分散させない

### MUST: `Validate()`メソッドまたは同等の検証APIを持つ

```go
func (x XxxType) Validate() error {
    return XxxSpec.IsSatisfiedBy(x)
}
```

## ドメインエラーパターン

### MUST: プロジェクト標準のドメインエラー契約を実装する

```go
type XxxError struct {
    reason string
    cause  error
}

var _ DomainError = (*XxxError)(nil) // コンパイル時チェック

func (e *XxxError) Error() string  { return e.reason }
func (e *XxxError) Reason() string { return e.reason }
func (e *XxxError) Unwrap() error  { return e.cause }
```

### 標準エラー型を優先する

- ValidationError
- NotFoundError
- ConflictError
- PermissionDeniedError

## ファイル構成ルール

### MUST: 1公開型 = 1ファイル（原則）

```
✅ email.go        → Email 型のみ
✅ member_id.go    → MemberID 型のみ
❌ value_objects.go → 複数の値オブジェクトをまとめない
```

例外：
- sealed interface + 閉じた実装群
- 公開型に密結合な private 実装

### MUST NOT: 曖昧なサフィックス

| 禁止 | 代替 |
|------|------|
| `XxxManager` | `XxxCoordinator`, `XxxRegistry` |
| `XxxUtil` | 具体責務を示す名前 |
| `XxxService` | 責務を明確化した名前 |
| `XxxFacade` | `XxxGateway`, `XxxAdapter` |

## エンティティ・集約パターン

### 集約インターフェース（例）

```go
type Xxx interface {
    ID() XxxID
    // getter
    // コマンドメソッド（必要に応じて Event / 新状態 / error を返す）
}
```

### 状態型パターン（任意）

状態（Created/Active/Deleted等）ごとに型を分け、状態遷移の不正を型で防ぐ。

## リポジトリインターフェースパターン

### CQRS分離（推奨）

```go
type XxxRepository interface {
    FindByID(ctx context.Context, id XxxID) (Xxx, error)
    Save(ctx context.Context, agg Xxx) error
}

type XxxQueryRepository interface {
    FindAll(ctx context.Context) ([]XxxDTO, error)
}
```

### MUST: モック生成手段を明示する

- `go:generate`、またはチーム標準のモック生成方式を採用する

## 不変性原則（Go）

### MUST: 破壊的変更より新しい値を返す

```go
func UpdateXxx(xxx Xxx, name string) Xxx {
    return Xxx{
        Name: name,
        Age:  xxx.Age,
    }
}
```
