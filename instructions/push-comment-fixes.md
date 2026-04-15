コメント対応の修正をリモートにpushせよ。

**やること:**
1. コミット済みの変更があるか確認する:
   ```
   git status
   git log --oneline origin/HEAD..HEAD
   ```
2. 未コミットの変更がある場合はコミットする:
   ```
   git add -A
   git commit -m "fix: address PR review comments"
   ```
3. リモートにpushする:
   ```
   git push
   ```
4. push結果を確認する

**重要:**
- push失敗時はABORTし、エラー詳細を出力する
- force pushは行わない
