# End Phase Git Commit Skill

## 目的
- runtime/commit_message.md を使用して git commit を実行する
- commit 成功後に runtime/commit_message.md と runtime/commit_details.md を空にする

## 起動条件
- End Phase のコミットメッセージ生成（runtime/commit_message.md 生成）が完了したとき
- 利用者が明示的に「コミットしない」と指示した場合は起動しない

## 前提
- 作業対象プロジェクトのルートでコマンドを実行する
- git が利用可能であること

## 入力
作業対象プロジェクト配下:

- runtime/commit_message.md

## 実行手順

1. 変更の有無を確認する

- 実行: git status --porcelain
- 出力が空なら「変更なし」として処理終了（commit は実行しない）

2. runtime/commit_message.md の存在を確認する

- 存在しない場合は失敗として中断し、理由を対話で明示する

3. ステージングを行う

- 実行: git add -A

4. コミットを実行する

- 実行: git commit -F runtime/commit_message.md

5. コミット関連ファイルを空にする

- runtime/commit_message.md を空文字で上書きする
- runtime/commit_details.md を空文字で上書きする
- 文字コードは UTF-8、改行は LF、末尾改行なしとする

## 失敗時

- git add / git commit が失敗した場合は処理を中断し、
  そのコマンドの標準エラー出力を対話で明示する
- 5 の書き込み失敗時は作業対象プロジェクト配下の runtime ディレクトリ作成を1回試行し、
  同一ファイルへの書き込みを1回再試行する
- 再試行でも失敗した場合は処理を中断し失敗理由を対話で明示する

## 禁止事項

- 5 以外の目的で runtime/commit_message.md を編集すること
- 5 以外の目的で runtime/commit_details.md を編集すること
- git reset / git revert / git rebase など履歴を書き換える操作
