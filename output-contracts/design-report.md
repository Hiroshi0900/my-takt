# Output Contract: design-report

## フォーマット

設計レポートは以下の構造で出力する。

---

# 設計レポート: {タスク名}

## 1. 設計サマリー

**変更の種類**: 値オブジェクト追加 / エンティティ変更 / ユースケース追加 / エラー型追加
**影響する層**: Domain / UseCase / Interface Adapter / Infrastructure
**新規ファイル数**: {数}
**変更ファイル数**: {数}

## 2. ドメイン型設計

### 新規値オブジェクト: {型名}

**ファイル**: `internal/{domain}/domain/{concept}.go`

```go
type {TypeName} string  // or appropriate base type

type {typeNameLower}Spec struct{}

func (s {typeNameLower}Spec) IsSatisfiedBy(v {TypeName}) error {
    // ビジネスルール
    if {条件} {
        return fmt.Errorf("{TypeName}: {エラーメッセージ}")
    }
    return nil
}

var {TypeName}Spec specification.Specification[{TypeName}] = {typeNameLower}Spec{}

func New{TypeName}(val string) {TypeName} { ... }
func SafeNew{TypeName}(val string) ({TypeName}, error) { ... }
func (v {TypeName}) Validate() error { ... }
```

**ビジネスルール**:
- {ルール1}
- {ルール2}

### 新規エンティティ / 集約変更

```go
// インターフェース変更・追加の場合
type {EntityName} interface {
    // 追加・変更するメソッド
    {NewMethod}() {ReturnType}
}
```

## 3. エラー型設計

### 新規エラー（必要な場合）

**ファイル**: `internal/{domain}/domain/errors.go`

```go
type {XxxError} struct {
    reason string
    cause  error
}

var _ domain.DomainError = (*{XxxError})(nil)

func New{XxxError}(reason string, cause error) *{XxxError} { ... }
func (e *{XxxError}) Error() string  { return e.reason }
func (e *{XxxError}) Reason() string { return e.reason }
func (e *{XxxError}) Unwrap() error  { return e.cause }
```

**HTTPマッピング**: {ステータスコード} ({理由})

## 4. リポジトリI/F設計

### コマンドリポジトリ

**ファイル**: `internal/{domain}/repository/{name}.go`

```go
//go:generate mockgen -source=$GOFILE -destination=../../test/mock/repository/{name}_mock.go -package=mock_repository

type {XxxRepository} interface {
    FindByID(ctx context.Context, id eventsourcing.AggregateID) ({Xxx}, error)
    Save(ctx context.Context, agg {Xxx}, events []eventsourcing.Event) error
    // 追加メソッド（必要な場合）
}
```

### クエリリポジトリ（必要な場合）

**ファイル**: `internal/{domain}/repository/{name}_query.go`

```go
type {XxxQueryRepository} interface {
    FindBy{Condition}(ctx context.Context, ...) (*{XxxDTO}, error)
}
```

## 5. ユースケースI/F設計

**ファイル**: `internal/{domain}/usecase/{name}.go`

```go
//go:generate mockgen -source=$GOFILE ...

type {XxxUseCase} interface {
    Execute(ctx context.Context, input {XxxInput}) (*{XxxOutput}, error)
}

type {XxxInput} struct {
    // プリミティブ型のみ使用
    Field1 string
    Field2 uint8
}

type {XxxOutput} struct {
    Result *{XxxDTO}
}
```

## 6. ファイル構成

### 新規作成ファイル

| ファイル | 内容 |
|--------|------|
| `{path}` | {説明} |

### 変更ファイル

| ファイル | 変更内容 |
|--------|--------|
| `{path}` | {変更の説明} |

## 7. 実装上の注意点

- {注意点1}
- {注意点2}

---

*設計完了。実装フェーズへ移行する。*
