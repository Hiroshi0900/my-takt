PRのコメントを収集し、対応優先度を分類せよ。

**やること:**
1. 対象PRを特定する:
   ```
   gh pr view --json number,url,headRefName
   ```
2. インラインコメント（レビューコメント）を収集する:
   ```
   gh api repos/{owner}/{repo}/pulls/{number}/comments
   ```
3. PR会話コメント（issueコメント）を収集する:
   ```
   gh api repos/{owner}/{repo}/issues/{number}/comments
   ```
4. botコメントとセルフレビューコメントを除外する（自分のコメントは対応対象外）
5. 各コメントを以下の3カテゴリに分類する:

   **対応必須 (required)**:
   - 正確性バグの指摘
   - 仕様不一致の指摘
   - セキュリティリスクの指摘
   - CI失敗の直接原因に関する指摘
   - 明確な「修正してください」「直してください」等のリクエスト

   **任意 (optional)**:
   - コードスタイル・命名の提案
   - 実装の好みに関する提案
   - リファクタリング提案
   - 「〜の方がいいかも」等の柔らかい提案

   **不要 (skip)**:
   - 既に解消済みのコメント
   - 古いdiffに対するコメント（コード既に変更済み）
   - 重複コメント
   - 質問のみ（コード変更不要）→ ただし回答は respond-pr-comments で行う

6. `comment-triage.md` に分類結果を出力する

**重要:**
- owner/repo は `gh repo view --json owner,name` で取得すること
- resolved済みのスレッドは対象外とする
- 分類に迷う場合は「対応必須」側に寄せる（安全側に倒す）
