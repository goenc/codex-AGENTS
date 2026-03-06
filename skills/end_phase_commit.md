# End Phase Commit Skill

## 目的
- End Phase のコミット関連ファイルを生成する

## 起動条件
- `skills/end_phase.md` から呼び出されたとき

## 出力対象

- コミット関連ファイルは作業対象プロジェクト配下の `runtime` ディレクトリで管理する
- 作業対象プロジェクト配下の `runtime` がない場合は作成してから書き込む

## 出力ファイル

### 1) コミット詳細ログ
出力先:
作業対象プロジェクト配下 runtime/commit_details.md

保存:
- 変更ありの場合のみ追記
- UTF-8 の LF 改行で保存する

内容:
- 日時（JST）
- 対象
- 変更
- 確認

変更は以下形式で記載:
・<変更内容を一文で記載>

### 2) 外部コミット用メッセージ
出力先:
作業対象プロジェクト配下 runtime/commit_message.md

保存:
- 常に上書き（変更の有無に関係なく毎回生成）
- 文字コードと改行は 1) と同一とする
- 末尾改行あり
- 全文を日本語で記載する

構造:
1行目: 日本語要約
2行目: 空行
3行目以降: 全角「・」で始まる箇条書き

生成:
- commit_details.md の全内容を元に作成
- 各ブロックの `summary` `code_changes` `verification` を抽出して反映する
- 抽出順は commit_details.md の出現順とし項目順は `summary` `code_changes` `verification` とする
- 形式:
  ・<変更内容を簡潔に一文で記載>

変更なしの場合:
- 1行目: 変更なし
- 箇条書きなし

## 実行順序

1. 変更の有無を判定する
2. 変更ありの場合のみ commit_details.md に追記する
3. 作業対象プロジェクト配下 runtime に commit_message.md を生成する

## 失敗時

- 作業対象プロジェクト配下の `runtime/commit_details.md` `runtime/commit_message.md` への書き込み失敗時は同プロジェクト配下の `runtime` ディレクトリ作成を1回試行したうえで同一ファイルへ1回再試行する
- 再試行でも失敗した場合は処理を中断し失敗理由を対話で明示する

## 禁止事項

- runtime 以外への保存
- commit_message.md の追記
- 日本語以外での記述
- 推測による補完
