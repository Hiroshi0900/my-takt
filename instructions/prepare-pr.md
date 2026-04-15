supervise通過後、PRを作成するための準備を行え。

**やること:**
1. SDDレポート群を読み込む:
   - `requirements.md` - 要件サマリー
   - `design.md` - 設計概要
   - `tasks.md` - タスク完了状況
   - `summary.md` - 監督フェーズのサマリー
2. `git diff main...HEAD --stat` で変更ファイル一覧と統計を取得する
3. `git log main..HEAD --format='%s'` でコミット履歴を取得する
4. PR title を生成する:
   - 70文字以内
   - conventional commit形式 (例: `feat: ユーザー認証APIの実装`)
   - コミット履歴とタスク内容から適切なプレフィックスを選択 (feat/fix/refactor/docs/chore)
5. PR body を生成する:
   - `## Summary` セクション: 変更の目的と概要 (3-5文)
   - `## Changes` セクション: 主要な変更点をバレットリストで記載
   - `## SDD Report` セクション: summary.md の内容を引用ブロックで埋め込み
   - `## Test Plan` セクション: 実施済みテストと検証方法
   - `## Breaking Changes` セクション: 破壊的変更があれば記載、なければ「なし」
6. 軽量セルフレビューを実行する:
   - `git diff main...HEAD` の全diffを確認し、以下をチェック:
     - デバッグコード残り (`console.log`, `fmt.Println` for debug, `debugger`, `TODO: remove` 等)
     - 機密情報 (APIキー、パスワード、トークンのハードコード)
     - 未コミットファイル (`git status` で確認)
     - 大きなバイナリファイルの誤コミット
   - 問題があれば severity (Critical/Warning) を付与して記録する
7. `08-pr-preparation.md` に出力する

**判定ルール:**
- 機密情報が検出された場合は「重大な問題あり」を選択する（Phase 4に戻す）
- デバッグコード残りは Warning として記録し、「準備完了」で続行可能
- 未コミットファイルが意図しないものの場合は「重大な問題あり」を選択する
