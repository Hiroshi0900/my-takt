**最初に `superpowers:finishing-a-development-branch` スキルを Skill ツールで invoke し、その4択提示手順に従え。**

実装バッチが完了した後、ブランチの終了方法をユーザーに選ばせて実行する。スキルの指示通り4オプション (merge / pr / keep / discard) を提示する。

**やること:**

1. Skill ツールで `superpowers:finishing-a-development-branch` を invoke する
2. 事前検証: `make test`, `make lint`, 該当時 `make build` を実行し pass を確認
   - いずれか fail の場合は **ここで中断**、`[FINISH: kept]` として後続を走らせない
3. スキルの指示通り、以下4オプションをユーザーに提示:
   - **merged**: ローカル main (or 指定ベース) にマージ、worktree 後片付け
   - **pr**: GitHub PR を作成 (既存 `create-pr-movement.md` / `prepare-pr.md` の既存手順を流用)、worktree 保持
   - **kept**: ブランチ保持のまま終了 (別作業継続想定)
   - **discarded**: ユーザーが `discard` と明示タイプした場合のみ削除実行
4. ユーザー選択に応じたアクションを実行
5. worktree の後片付けは merge / discard のみ (keep / pr では保持)
6. 出力契約 `sp-finish-result` に従いレポート

**非対話モード (fully-auto) の場合:**

- デフォルトは `[FINISH: pr]` (既存 SDD の挙動に最寄り)
- PR 作成には既存 `prepare-pr` / `create-pr-movement` / `post-pr-summary` のレポートを参照できる

**判定:**

- merge / pr / keep / discard のいずれかを完遂 → 対応タグ
- 検証 fail で中断 → `[FINISH: kept]`
