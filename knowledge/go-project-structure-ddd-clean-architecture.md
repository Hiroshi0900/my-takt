# Go DDD + Clean Architecture プロジェクト構造（汎用）

## 概要

Goで実装された DDD + Clean Architecture プロジェクトの一般的な構成例。
プロジェクト固有の命名やフレームワークは差し替えて使う。

## ディレクトリ構成（例）

```
project/
├── cmd/
│   └── server/            # エントリーポイント / composition root
├── internal/
│   ├── domain/            # 共通ドメインプリミティブ（任意）
│   ├── usecase/           # 共通ユースケースエラー型など（任意）
│   ├── infrastructure/    # DB, API client, logger 等
│   ├── interface/         # HTTP/GraphQL/gRPC handler 等（任意）
│   └── {domain}/          # 各ドメインモジュール
└── migrations/            # DBマイグレーション（任意）
```

## 各ドメインモジュールの構造（例）

```
internal/{domain}/
├── domain/
│   ├── {entity}.go
│   ├── {value_object}.go
│   ├── {spec}.go
│   └── factory.go         # 任意
├── usecase/
│   ├── {usecase}.go       # I/F + Input/Output
│   └── dto.go             # DTO（任意）
├── repository/
│   ├── {repo}.go          # コマンドリポジトリI/F
│   └── {repo}_query.go    # クエリリポジトリI/F（任意）
├── interface_adapter/
│   ├── usecase/           # ユースケース実装
│   │   ├── {usecase}.go
│   │   └── converter.go   # DTO変換（任意）
│   └── repository/
│       └── {backend}/     # 実装（postgres, mysql, memory等）
└── test/
    └── mock/              # 生成モック（任意）
```

## Clean Architecture の層

### domain層
- もっとも内側
- ビジネスルールを表現
- インフラ詳細を知らない

### usecase層
- アプリケーションのユースケースを表現
- domain を使って処理を編成
- リポジトリ契約を定義

### interface_adapter層
- 外部入出力と usecase/domain の橋渡し
- DTO変換、入力検証、レスポンス変換

### infrastructure層
- DB / 外部API / メッセージング / ログなどの実装
- composition root で依存注入

## 主要パターン（例）

### 値オブジェクトの2関数パターン（任意）

```go
func NewXxx(val string) XxxType { ... }      // 内部向け
func SafeNewXxx(val string) (XxxType, error) // 外部入力向け
```

### ユースケース実装構造（例）

```go
type xxxUseCaseImpl struct {
    repo repository.XxxRepository
}

func NewXxxUseCase(repo repository.XxxRepository) usecase.XxxUseCase {
    return &xxxUseCaseImpl{repo: repo}
}
```

### エラー変換フロー（例）

```
domain error
  ↓
usecase error（文脈付与）
  ↓
transport error mapping（HTTP/gRPC/GraphQL）
  ↓
外部向けレスポンス
```
