# Instruction: refactor-investigate-structure

既存コードベースの構造問題を調査し、リファクタリング判断に必要な証拠を集めよ。

## やること

1. 対象ディレクトリまたはモジュールを特定する
2. ファイル構造を把握する
3. 構造問題の候補を列挙する
4. 問題ごとに根拠ファイルを集める
5. 実行前提の不足を確認する

## 重点観点

- 循環依存の兆候
- 神モジュール、神ディレクトリ
- 関連責務の散在
- 同型 workflow や rule / movement の重複
- 共通コア化できそうな instruction / output-contract / movement
- 過度なネスト
- `entities/`, `services/`, `repositories/` など技術駆動パッケージ
- 変更影響範囲の広さ
- テスト、lint、build など安全装置の有無

## 出力要求

`refactor-analysis.md` に、問題一覧、根拠、推奨候補、保留理由を整理すること。
