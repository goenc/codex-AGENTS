# End Phase Git Push Skill

## 目的
- `work` ブランチを `origin/work` へ push する
- 初回 push 時は upstream を設定する
- `main` ブランチには push しない

## 起動条件
- `skills/end_phase_git_commit.md` の完了後
- 利用者が明示的に「プッシュしない」と指示した場合は起動しない

## 前提
- 作業対象プロジェクトのルートでコマンドを実行する
- git が利用可能であること
- push 対象ブランチは `work` のみとする

## 実行手順
1. 現在ブランチを確認する
- 実行: `git branch --show-current`
- 出力が `work` でない場合は失敗として中断し、理由を対話で明示する

2. upstream 設定の有無を確認する
- 実行: `git rev-parse --abbrev-ref --symbolic-full-name @{u}`

3. push を実行する
- 2 が成功した場合:
  - 実行: `git push`
- 2 が失敗した場合:
  - 実行: `git push -u origin work`

## 失敗時
- `git push` が失敗した場合は処理を中断し、そのコマンドの標準エラー出力を対話で明示する

## 禁止事項
- `main` ブランチへの push
- `git push --force`
- `git push --force-with-lease`
- 利用者の明示なしに push 先を `origin/work` 以外へ変更すること