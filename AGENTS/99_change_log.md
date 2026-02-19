# AGENTS Change Log

## Records
- `change_id`: `CHG-20260220-0609`
- `target_files`: `AGENTS.md`, `AGENTS/15_operation_gate_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`, `AGENTS/98_maintenance_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-INDEX-IMPL-001..005`, `RULE-OG-IMPL-001..004`, `RULE-PROJ-PLAN-001..007`, `RULE-SPEECH-001..004`, `RULE-DEV-WORKLOG-002..015`, `RULE-DEV-IMPL-001..004`, `RULE-DEV-VERIFY-001..005`, `RULE-DEV-BUILD-001..003`, `RULE-DEV-FONT-001..005`, `RULE-DEV-LOOP-007..009`, `RULE-OUT-IMPL-001..003`, `RULE-OUT-ASK-001..004`, `RULE-MAINT-IMPL-001..002`, `RULE-MAINT-002`
- `validation_result`: `PASS`（参照先存在・見出し存在・クロスファイル重複IDなし・優先順位に「やり取り回数を減らす観点」を追加）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\snapshots\20260220-060435\`
- `notes`: `実装専用運用へ寄せるため、実装意図検知時の即実装、Fast Path（Light Verify + 1コミット）既定化、readaloud/WORKLOGの条件付き化、質問最小化原則を反映`

- `change_id`: `CHG-20260220-0516`
- `target_files`: `AGENTS.md`, `AGENTS/10_project_rules.md`, `AGENTS/15_operation_gate_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-PROJ-CONTEXT-001..004`, `RULE-PROJ-PLAN-001..005`, `RULE-SPEECH-001..004`, `RULE-DEV-COMMIT-001..008`, `RULE-DEV-WORKLOG-001..015`, `RULE-DEV-RESULT-001..005`, `RULE-OUT-COST-003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260220-0516\`
- `notes`: `運用ゲート専用ファイル AGENTS/15_operation_gate_rules.md を新設し、再開時コンテキスト/実装モード判定/発話/event.json/コミット&worklog/result 出力ルールを移管して集約`

- `change_id`: `CHG-20260220-0508`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-DEV-COMMIT-008`, `RULE-DEV-WORKLOG-013..015`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260220-0508\`
- `notes`: `コミット前の固定ゲート（Commit Preflight）を追加し、本文必須・staged突合・scope整合がPASSでなければコミット不可に強化`

- `change_id`: `CHG-20260219-2302`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-DEV-WORKLOG-001..012`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-2302\`
- `notes`: `CODEX_WORKLOG.md をコミット間ログの単一出力先として固定し、staged差分突合・AGENT/SOFT混在時の分割コミット・コミット後リセット手順を追加`

- `change_id`: `CHG-20260219-2245`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-DEV-COMMIT-005..007`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-2245\`
- `notes`: `コミットメッセージ書式を固定化（1行目タイトルのみ、2行目以降は行頭「・」で列挙）`

- `change_id`: `CHG-20260219-0454`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-PROJ-CONTEXT-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0454\`
- `notes`: `要件定義書に target_project_root の必須記載を追加し、再開ターンでの一致確認を固定化`

- `change_id`: `CHG-20260219-0359`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`, `AGENTS/_snapshots/rollback_latest.txt`
- `rules_added_or_updated`: `RULE-DEV-COMMIT-ROUTE-001..007`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0359\`
- `notes`: `コミット指示の誤字許容ルーティングを追加し、エージェント系ヒント同時成立時はAGENTS管理リポジトリへ振り分け`

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

- `change_id`: `CHG-20260218-2110`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-DEV-CONFIG-007..012`, `RULE-DEV-RESULT-002`, `RULE-DEV-RESULT-005`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260218-2110\`
- `notes`: `外部設定形式の実装前宣言・静的検査・違反時停止・最終報告必須を追加し、minimal result.json に config_policy_validation を必須化`

- `change_id`: `CHG-20260218-2119`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-PROJ-CONFIG-007..009`, `RULE-DEV-CONFIG-008..009`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260218-2119\`
- `notes`: `外部設定ポリシーをJSON専用へ明文化し、.txtを設定扱い禁止、Step4検証をdefault/override JSON契約と非JSON違反検出へ更新`

- `change_id`: `CHG-20260219-0016`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`, `KNOWLEDGE.md`
- `rules_added_or_updated`: `RULE-PROJ-KB-001..005`, `RULE-DEV-KB-001..003`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0016\`
- `notes`: `共通ナレッジ原本を KNOWLEDGE.md に固定し、タグ一致参照と手動テストOK後の追記更新ルールを追加`

- `change_id`: `CHG-20260219-0128`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-DEV-COMMIT-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0128\`
- `notes`: `コミットメッセージを日本語固定とし、実装コミットでは要件定義更新の記載を不要化（文書のみコミットは記載可）`

- `change_id`: `CHG-20260219-0133`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-DEV-LOOP-009`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0133\`
- `notes`: `User Manual GateがOKのサイクルでは、詳細設計更新をコミット前必須に強化`

- `change_id`: `CHG-20260219-0143`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-DEV-LOOP-009`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0143\`
- `notes`: `詳細設計更新ルールをコミット前のみへ修正し、OK判定履歴の有無による条件分岐を削除`

- `change_id`: `CHG-20260219-0208`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-DEV-LOOP-007`, `RULE-DEV-LOOP-009`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0208\`
- `notes`: `要件定義更新と詳細設計更新の両方をコミット直前必須へ強化`

- `change_id`: `CHG-20260219-0340`
- `target_files`: `AGENTS/10_project_rules.md`, `AGENTS/99_change_log.md`
- `rules_added_or_updated`: `RULE-PROJ-SPECDET-001..006`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\RustProjects\AGENTS\_snapshots\CHG-20260219-0340\`
- `notes`: `要件定義初期作成時に再現性固定仕様（UI定数・イベント順・JSON運用・表示優先）の明記を必須化`
