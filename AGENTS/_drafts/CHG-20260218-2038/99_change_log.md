# AGENTS Change Log

## Records
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

- `change_id`: `CHG-20260218-2027`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-PROJ-PLAN-002..004`, `RULE-DEV-TARGETFILES-005..006`, `RULE-SPEECH-003`, `RULE-DEV-RESULT-001..004`, `RULE-OUT-COST-003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260218-2027\`
- `notes`: `実装モードの適用ゲートを単一化し、Speech適用境界を明確化、対象ファイル列挙に上限を導入、result.jsonをminimal/fullの2モード化`

- `change_id`: `CHG-20260218-2038`
- `target_files`: `AGENTS.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`, `AGENTS/98_maintenance_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-MAINT-001..005`, `RULE-DEV-TARGETFILES-002..004`, `RULE-DEV-RESULT-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260218-2038\`
- `notes`: `Change Recordを99へ分離、Hardeningを98へ分離、result.jsonをminimal固定へ簡素化、TargetFiles定義を強制除外+上限+優先順へ短文化`
