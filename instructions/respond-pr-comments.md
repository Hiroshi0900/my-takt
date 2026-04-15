comment-triage.md の分類結果に基づき、各PRコメントに返信を投稿せよ。

**やること:**
1. `comment-triage.md` を読み込み、全コメントの分類結果を確認する
2. owner/repoを取得する:
   ```
   gh repo view --json owner,name
   ```
3. 各コメントへの返信を投稿する:

   **対応必須 (required) → 修正済みの場合:**
   ```
   gh api repos/{owner}/{repo}/pulls/{number}/comments/{comment_id}/replies \
     --method POST --field body="修正しました。{修正内容の簡潔な説明}

   変更箇所: \`{ファイル名}:{行番号}\`"
   ```

   **任意 (optional) → 対応した場合:**
   ```
   gh api repos/{owner}/{repo}/pulls/{number}/comments/{comment_id}/replies \
     --method POST --field body="ご提案ありがとうございます。対応しました。"
   ```

   **任意 (optional) → 見送る場合:**
   ```
   gh api repos/{owner}/{repo}/pulls/{number}/comments/{comment_id}/replies \
     --method POST --field body="ご提案ありがとうございます。今回は以下の理由で見送りとさせてください:
   {見送り理由}"
   ```

   **不要 (skip) → 質問への回答が必要な場合:**
   - 質問に対して簡潔に回答する

4. PR会話コメントへの返信は `gh pr comment` を使用する:
   ```
   gh pr comment {number} --body "{返信内容}"
   ```

**重要:**
- 返信は簡潔かつ丁寧に（1-3文程度）
- 技術的な根拠がある場合は簡潔に理由を添える
- 返信失敗時はエラーを記録してABORTする（手動対応を促す）
