# AGENTS Split: Output Rules

## Output Policy (User Preference)
ユーザー方針に合わせ、出力は「最小・明確・実行判断に必要な情報のみ」を既定とする。

運用ルール:
- 途中経過の逐次報告は原則省略し、最終報告を中心に出力する。
- 最終報告の基本フォーマットは次の順序とする:
  1. 結論（何をしたか / していないか）
  2. 変更ファイル
  3. テスト結果
  4. 残リスク / 未実施事項
  5. 次アクション（必要時のみ）
- コマンドログ、差分全文、長い説明はユーザー要求時のみ提示する。
- 失敗時は「失敗点」「原因候補」「再実行方針」の3点のみ簡潔に示す。
- 質問への回答のみを求められた場合は、原則3行以内で答える。

## Minimal Output With Safety Exceptions
結果中心の最小出力を既定とし、安全性を損なう場合のみ短い補足を許可する。

- `RULE-OUT-MIN-001` 手順説明、内部思考、長い判断理由は原則出力しない。
- `RULE-OUT-MIN-002` 通常出力は次の3項目に限定する: 変更点 / 実行結果 / 残課題。
- `RULE-OUT-MIN-003` 重大バグの疑い、仕様不明確のまま実装すると危険、再現不能または非決定的不具合の対応時は、短い説明根拠の出力を許可する。
- `RULE-OUT-MIN-004` 例外説明を出す場合でも、行数は最小化し、追加で求められるまで詳細展開しない。

## AGENTS Change Record
- `change_id`: `CHG-20260215-1534`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-OUT-MIN-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260215-1534\`
- `notes`: `結果中心・説明最小を既定化し、安全性例外の最小根拠出力を追加`


