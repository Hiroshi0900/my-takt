08-pr-preparation.md を元にPRを作成せよ。

**やること:**
1. `08-pr-preparation.md` を読み込み、PR title と PR body を取得する
2. 現在のブランチ名を `git branch --show-current` で確認する
3. リモートにプッシュする:
   ```
   git push -u origin <branch>
   ```
4. 既存PRの有無を確認する:
   ```
   gh pr view --json number,url 2>/dev/null
   ```
5. PRが存在しない場合、新規作成する:
   ```
   gh pr create --title "<title>" --body "<body>"
   ```
6. PRが既に存在する場合、bodyを更新する:
   ```
   gh pr edit <number> --body "<body>"
   ```
7. PR URL と番号を `09-pr-created.md` に記録する

**重要:**
- `gh pr create` の `--body` にはヒアドキュメントを使用して改行を正しく渡すこと
- push失敗時はエラー詳細をappendixに記載してABORTすること
- PRのbase branchはmainとする（デフォルト）
