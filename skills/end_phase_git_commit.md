# End Phase Git Commit Skill

## 目的
- 現在の HEAD が main でも main の履歴を変えず、`work` ブランチへ切り替えて commit を実行する
- 作業対象プロジェクト配下の `runtime/commit_message.md` を使用して git commit を実行する
- commit 成功後に `runtime/commit_message.md` と `runtime/commit_details.md` を空にする

## 起動条件
- End Phase のコミットメッセージ生成（`runtime/commit_message.md` 生成）が完了したとき
- 利用者が明示的に「コミットしない」と指示した場合は起動しない

## 前提
- 作業対象プロジェクトのルートでコマンドを実行する
- git が利用可能であること
- `main` ブランチの履歴は変更しない
- `work` ブランチが存在しない場合は、現在の HEAD を起点として `work` ブランチを新規作成してよい

## 入力
作業対象プロジェクト配下:
- `runtime/commit_message.md`

## 実行手順
1. 現在ブランチを確認する
- 実行: `git branch --show-current`

2. `work` ブランチの存在を確認する
- 実行: `git show-ref --verify --quiet refs/heads/work`

3. `work` ブランチへ切り替える
- 2 が成功した場合:
  - 実行: `git switch work`
- 2 が失敗した場合:
  - 実行: `git switch -c work`
- どちらの場合も、現在の `main` ブランチには commit を積まない

4. 変更の有無を確認する
- 実行: `git status --porcelain`
- 出力が空なら「変更なし」として処理終了する
- この場合、git commit は実行しない

5. `runtime/commit_message.md` の存在を確認する
- 存在しない場合は失敗として中断し、理由を対話で明示する

6. ステージングを行う
- 実行: `git add -A`

7. コミットを実行する
- 実行: `git commit -F runtime/commit_message.md`

8. コミット関連ファイルを空にする
- `runtime/commit_message.md` を空文字で上書きする
- `runtime/commit_details.md` を空文字で上書きする
- 文字コードは UTF-8、改行は LF、末尾改行なしとする

## 失敗時
- `git switch` `git add` `git commit` が失敗した場合は処理を中断し、そのコマンドの標準エラー出力を対話で明示する
- 8 の書き込み失敗時は作業対象プロジェクト配下の `runtime` ディレクトリ作成を1回試行し、同一ファイルへの書き込みを1回再試行する
- 再試行でも失敗した場合は処理を中断し失敗理由を対話で明示する

## 禁止事項
- 8 以外の目的で `runtime/commit_message.md` を編集すること
- 8 以外の目的で `runtime/commit_details.md` を編集すること
- `main` ブランチ上で commit を実行すること
- `git reset` `git revert` `git rebase` など履歴を書き換える操作