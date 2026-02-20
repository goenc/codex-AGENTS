# AGENTS Change Log

## Scope
このファイルは共通母艦（AGENTS群）の変更履歴のみ記録する。
プロジェクト固有の実装ログ、運用ログ、障害ログは記録しない。

## Records
- change_id: CHG-20260220-1905
- target_files: AGENTS.md, AGENTS/20_development_rules.md, AGENTS/99_change_log.md
- rules_added_or_updated: RULE-DEV-BUILDWIN-001..006
- validation_result: PASS（重複IDなし、Development Rules配下に追記、Windowsビルド時のexeロック防止運用を明文化）
- rollback_point: none
- notes: ビルド前に毎回プロセス終了試行、フルパス一致推奨、対象なしは無害続行、同一ユーザー権限前提、`pwsh -File build.ps1` 実行例を追加

- change_id: CHG-20260221-0007
- target_files: AGENTS/20_development_rules.md, AGENTS/99_change_log.md
- rules_added_or_updated: RULE-DEV-COMMITMSG-001..004
- validation_result: PASS（重複IDなし、適用箇所はDevelopment Rules内、既存ルール意味変更なし）
- rollback_point: none
- notes: commit message仕様を日本語2層構造「- 何を:」のみに固定

- change_id: CHG-20260220-1743
- target_files: AGENTS/15_operation_gate_rules.md, AGENTS/99_change_log.md
- rules_added_or_updated: RULE-OG-STARTUP-010
- validation_result: PASS
- rollback_point: none
- notes: project識別子不一致防止

- change_id: CHG-20260220-1735
- target_files: AGENTS.md, AGENTS/10_project_rules.md, AGENTS/15_operation_gate_rules.md, AGENTS/20_development_rules.md, AGENTS/30_output_rules.md, AGENTS/98_maintenance_rules.md, AGENTS/99_change_log.md
- rules_added_or_updated: RULE-INDEX-IMPL-008, RULE-PROJ-ARTIFACT-001..006, RULE-PROJ-SOT-001..004, RULE-PROJ-ENTRY-001..003, RULE-OG-STARTUP-001..009, RULE-OG-INTENT-001..002, RULE-DEV-FLOW-001..006, RULE-DEV-SOURCE-001..003, RULE-DEV-VERIFY-001..004, RULE-DEV-PROMOTE-001..003, RULE-OUT-001..006, RULE-MAINT-001..006
- validation_result: PASS（旧仕様文書依存の削除、SoT置換、起動ゲート置換、重複IDなし）
- rollback_point: none
- notes:
  - 削除: 旧仕様文書必須、旧仕様文書が唯一要件、作業ルートを旧仕様文書から読む運用。
  - 置換: Operation Gateを `再開_<project>.md` + `不変条件_<project>.md` + source/entry_points 存在確認へ変更。
  - 置換: Single Source of Truthを「ソース仕様 / 不変条件契約 / 再開ファイル探索制御」の3軸へ変更。
  - 置換: 開発フローを「変更要求(req) -> 最小実装 -> 軽量検証 -> コミット」へ変更。
  - 追加: 入力崩れの自動整形継続、`unknown` 明示、small reqのみ `1req=1commit`、large req分割運用。
