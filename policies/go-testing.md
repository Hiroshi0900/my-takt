# Policy: go-testing

## 適用範囲

Goプロジェクトのテストコード（`*_test.go`）に適用する。

## テストファイル命名と配置

### MUST: 実装に近い場所へ配置（単体テスト）

```
✅ domain/xxx_test.go
✅ interface_adapter/usecase/xxx_test.go
✅ internal/test/integration/...   # 統合テスト（任意）
```

### MUST: テスト関数命名パターン

```go
func TestXxxType_MethodName(t *testing.T) { ... }
func TestXxxUseCaseImpl_Execute(t *testing.T) { ... }
```

## モック利用

### MUST: 外部依存をユニットテストへ持ち込まない

- DB, HTTP, Queue, Cloud SDK はモックまたはスタブを使う
- 実インフラ確認は統合テストに分離する

### 推奨: モック生成を自動化

```go
//go:generate mockgen -source=$GOFILE -destination=../../test/mock/repository/xxx_mock.go -package=mock_repository
```

## ユースケーステストの必須ケース

```
✅ 正常系（有効入力）
✅ 入力バリデーション異常系
✅ リポジトリ/依存エラー異常系
✅ ドメインルール違反
✅ 境界値
```

## 値オブジェクトテスト

### MUST: 有効値・無効値・境界値を検証する

```go
func TestSafeNewXxx(t *testing.T) {
    // valid / invalid / boundary
}
```

## アサーション

### チーム標準を統一する

- `testify/assert` や `require` など、プロジェクト標準を一貫して使う
- 致命的前提チェックには `require` を使い、それ以外は `assert` を使う
