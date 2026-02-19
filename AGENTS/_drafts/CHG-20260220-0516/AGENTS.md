# AGENTS.md (Index)

## Scope
このファイルは `C:\Users\gonec\RustProjects` とその配下に適用する。
より深い階層に別の `AGENTS.md` がある場合はそちらを優先する。

## Split Structure
この `AGENTS.md` はインデックス専用とし、実行ルール本体は以下の分割ファイルに定義する。

読み込み順（上から順に適用）:
1. `AGENTS/10_project_rules.md`
2. `AGENTS/15_operation_gate_rules.md`
3. `AGENTS/20_development_rules.md`
4. `AGENTS/30_output_rules.md`
5. `AGENTS/98_maintenance_rules.md`

変更履歴:
- `AGENTS/99_change_log.md`（記録専用。実行ルールとしては適用しない）

## Safe Operation Rules
- ルールの重複記載を禁止する（1つのルールは1ファイルにのみ記載）。
- 競合時は、後段ファイルより前段ファイルを優先するのではなく、`AGENTS/20_development_rules.md` の `Priority Order` を唯一の判定基準とする。
- 分割ファイルの参照に失敗した場合は作業を開始せず、失敗点と再実行方針のみ報告する。
- ルール追加・変更時は、該当分割ファイルを更新し、変更履歴は `AGENTS/99_change_log.md` に追記する。
