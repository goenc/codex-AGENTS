# AGENTS Split: Maintenance Rules

## AGENTS Update Hardening
`AGENTS.md` / `AGENTS/` 配下を更新する場合のみ、本節を適用する。

- `RULE-MAINT-001` 追加・変更するルールには一意IDを付与する。
- `RULE-MAINT-002` 1つのルールは1ファイルにのみ記載し、重複転記を禁止する。
- `RULE-MAINT-003` 反映前に、参照先存在・重複ID・優先順位矛盾・旧仕様文書依存語の残存を確認する。
- `RULE-MAINT-004` ルール更新と同一コミットで `AGENTS/99_change_log.md` を更新する。
- `RULE-MAINT-005` `AGENTS/99_change_log.md` には共通母艦（AGENTS群）の変更のみ記録し、プロジェクト固有ログを記録しない。
- `RULE-MAINT-006` 大規模変更時はロールバックポイントを `AGENTS/99_change_log.md` に残す。
