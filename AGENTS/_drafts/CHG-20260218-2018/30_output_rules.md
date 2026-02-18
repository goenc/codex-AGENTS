# AGENTS Split: Output Rules

## Output Policy (User Preference)
ユーザー方針に合わせ、出力は「最小・明確・実行判断に必要な情報のみ」を既定とする。

運用ルール:
- 途中経過の逐次報告は原則省略し、最終報告を中心に出力する。
- 最終報告の基本フォーマットは次の順序とする:
  1. 結論（何をしたか / していないか）
  2. 実行結果（必要な範囲のみ）
  3. 残課題（ある場合のみ）
- 変更ファイル、テスト結果、次アクションの詳細展開はユーザー要求時または失敗時のみ行う。
- コマンドログ、差分全文、長い説明はユーザー要求時のみ提示する。
- 失敗時は「失敗点」「原因候補」「再実行方針」の3点のみ簡潔に示す。
- 質問への回答のみを求められた場合は、原則3行以内で答える。

## Minimal Output With Safety Exceptions
結果中心の最小出力を既定とし、安全性を損なう場合のみ短い補足を許可する。

- `RULE-OUT-MIN-001` 手順説明、内部思考、長い判断理由は原則出力しない。
- `RULE-OUT-MIN-002` 通常出力は次の3項目に限定する: 変更点 / 実行結果 / 残課題。
- `RULE-OUT-MIN-003` 重大バグの疑い、仕様不明確のまま実装すると危険、再現不能または非決定的不具合の対応時は、短い説明根拠の出力を許可する。
- `RULE-OUT-MIN-004` 例外説明を出す場合でも、行数は最小化し、追加で求められるまで詳細展開しない。

## Output Cost Guardrails
使用量増加を抑えるため、通常出力の長さと展開条件を固定する。

- `RULE-OUT-COST-001` 通常応答は原則3-8行に収める。
- `RULE-OUT-COST-002` 詳細テンプレート（変更ファイル一覧、テスト内訳、次アクション）は、ユーザー明示要求または失敗時のみ展開する。
- `RULE-OUT-COST-003` 開始時/終了時の読み上げ必須要件は `AGENTS/20_development_rules.md` の `RULE-SPEECH-001..002` を優先し、本ファイルの最小出力制限で抑制しない。
- `RULE-OUT-COST-004` 通常応答は固定3行テンプレート（`結論` `実行結果` `残課題`）を既定とし、追加行は最大2行までに制限する。
- `RULE-OUT-COST-005` 固定テンプレートに収まらない詳細（変更ファイル一覧、テスト詳細、次アクション候補）は、ユーザー明示要求または失敗時のみ展開する。

## AGENTS Change Record
- `change_id`: `CHG-20260215-1534`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-OUT-MIN-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260215-1534\`
- `notes`: `結果中心・説明最小を既定化し、安全性例外の最小根拠出力を追加`
- `change_id`: `CHG-20260216-1750`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-OUT-COST-001..003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260216-1750\`
- `notes`: `通常出力の短文化と詳細展開条件を明確化し、使用量増加を抑制`
- `change_id`: `CHG-20260216-1800`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-OUT-COST-004..005`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260216-1800\`
- `notes`: `通常応答の固定テンプレート化で出力量の上振れを抑制`
- `change_id`: `CHG-20260217-2209`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-SPEECH-001..002`, `RULE-DEV-LOOP-007..008`, `RULE-OUT-COST-003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260217-2209\`
- `notes`: `RULE-SPEECH参照を実体化し、Mandatory Development LoopのStep 7欠落を補完、rollback_pointのパスを現行ワークスペースへ統一`
- `change_id`: `CHG-20260217-2245`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-DEV-BUILD-001..003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260217-2245\`
- `notes`: `実装サイクル完了時に cargo build を必須化し、手動テスト用実行ファイルのパス報告と result.json 記録を追加`
- `change_id`: `CHG-20260217-2318`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-PROJ-FONT-001..009`, `RULE-DEV-FONT-001..005`, `RULE-DEV-TARGETFILES-002`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260217-2318\`
- `notes`: `フォント運用ポリシーを追加し、未指定時NotoSansJP固定、Windowsフォント直参照/.ttc/デフォルト依存の機械検出と未遵守時Fail条件を追加`
- `change_id`: `CHG-20260218-2018`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-PROJ-CONFIG-001..006`, `RULE-DEV-CONFIG-001..006`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260218-2018\`
- `notes`: `外部設定値の単一原本運用を追加し、target配下をコピー先限定、実行時overrideの保存先と反映条件（明示操作のみ）を固定`

