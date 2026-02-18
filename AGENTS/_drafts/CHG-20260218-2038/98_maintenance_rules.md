# AGENTS Split: Maintenance Rules

## AGENTS Update Hardening
`AGENTS.md` / `AGENTS/` 配下を更新する場合のみ、本節を適用する。

- `RULE-MAINT-001` (`HARD-001`) 追加・変更するルールには一意ID（例: `RULE-OUT-001`）を付ける。
- `RULE-MAINT-002` (`HARD-002`) 直接上書きせず、草案作成 -> 検証通過 -> 反映の順で更新する。
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
