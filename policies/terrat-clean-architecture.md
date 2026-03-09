# Policy: go-clean-architecture

## 適用範囲

Goで実装された DDD + Clean Architecture プロジェクトのコード変更に適用する。

## 層の依存ルール

```
domain層 ─→ 外側の層に依存しない（標準ライブラリ + プロジェクト内の純粋ユーティリティのみ）
usecase層 ─→ domain層のみ（アプリケーションサービス / オーケストレーション）
interface_adapter層 ─→ usecase層 + domain層（変換・入出力・実装）
infrastructure層 ─→ すべての層（DB, HTTP client, Logger, Queue等）
composition root ─→ すべての層（DI wiring）
```

## MUST（必須ルール）

### domain層
- `database/sql`, `net/http`, AWS/GCP SDK, DIフレームワーク等のインフラ依存を持ち込まない
- ビジネスルールは domain 内に閉じ込める
- `Validate() error` を持つ値型は、生成時に必ず検証される構造にする

### usecase層
- リポジトリインターフェースは usecase層（または同等のアプリケーション境界）に置く
- Input/Output は外部境界に適した型（プリミティブやDTO）を優先する
- domainエラーは usecase レベルで文脈付きにラップする

### interface_adapter層
- ハンドラー/コントローラは domain オブジェクトの生成と検証を安全なコンストラクタ経由で行う
- DTO変換は特定の変換レイヤー（converter/mapper）に集約する
- ハンドラーが domain の内部構造に過度に依存しない

### infrastructure層
- 技術的詳細（DB接続、外部APIクライアント、ログ、メッセージング）を担当する
- composition root で DI wiring を行う

## MUST NOT（禁止事項）

- domain層に DB/HTTP/クラウドSDK の import を書く
- ハンドラーから直接 domain の危険なコンストラクタ（panic前提等）を呼ぶ
- ユースケース実装を domain層に置く
- リポジトリI/Fを infrastructure層に置く（外側都合で内側契約を定義しない）
- domainエラーをそのままHTTPレスポンスに漏らす（変換レイヤーを通す）

## エラー変換チェックリスト

新しいドメインエラーを追加した場合、以下を確認：

1. ドメインエラー型がプロジェクトの標準エラー契約を実装している
2. コンパイル時チェック（interface 実装確認）がある
3. 境界変換（HTTP/GraphQL/gRPC等）にマッピングが追加されている
4. ステータスコード / エラーコード / メッセージが妥当

## ファイル配置チェックリスト

新しいユースケースを追加する場合（例）：

```
✅ {domain}/usecase/{name}.go                  # インターフェース + Input/Output
✅ {domain}/repository/{name}.go               # リポジトリI/F（必要なら）
✅ {domain}/interface_adapter/usecase/{name}.go  # 実装
✅ {domain}/interface_adapter/usecase/converter.go # DTO変換（更新）
✅ composition root（main.go / wire.go 等）        # DI wiring 追加
```
