# AGENTS Split: Maintenance Rules

## Interaction-Minimization Priority
AGENTS更新時も安全性を保ったまま往復を減らす。

- `RULE-MAINT-IMPL-001` 本ファイルは `AGENTS.md` の `RULE-INDEX-IMPL-001..007` を参照し、運用儀式は必要最小限にする。
- `RULE-MAINT-IMPL-002` 破壊的変更を伴わない小変更は、最小手順（スナップショット -> 検証 -> 反映）を許可する。

## AGENTS Update Hardening
`AGENTS.md` / `AGENTS/` 配下を更新する場合のみ、本節を適用する。

- `RULE-MAINT-001` (`HARD-001`) 追加・変更するルールには一意ID（例: `RULE-OUT-001`）を付ける。
- `RULE-MAINT-002` (`HARD-002`) 更新手順は既定で `スナップショット -> 検証通過 -> 反映` とし、大規模変更時のみ `草案作成 -> 検証通過 -> 反映` を必須化する。
- `RULE-MAINT-003` (`HARD-003`) 反映前に、参照先存在・見出し・重複ルール・優先順位矛盾を確認する。
- `RULE-MAINT-004` (`HARD-004`) 反映前にスナップショット（日時付き）を保存する。
- `RULE-MAINT-005` (`HARD-005`) 直前世代へ戻す手順を保持し、失敗時は即復旧する。

## Change Template
更新時は次の項目を `AGENTS/99_change_log.md` に追記する。

- `change_id`: `CHG-YYYYMMDD-HHMM`
- `target_files`: 変更対象ファイル一覧
- `rules_added_or_updated`: ルールID一覧
- `validation_result`: `PASS` / `FAIL`（理由）
- `rollback_point`: 復旧元スナップショット
- `notes`: 注意点
