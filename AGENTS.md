# AGENTS.md (Index)

## Scope
このファイルは `C:\Users\gonec\RustProjects` とその配下に適用する。
より深い階層に別の `AGENTS.md` がある場合はそちらを優先する。

## Ultra Fast Path Policy
やり取り回数とベースコストを削減し、1回の指示で完走しやすくするための補助原則を定義する。

- `RULE-INDEX-IMPL-001` 優先順位は `安全性（破壊的変更確認のみ） > 実装完走 > やり取り削減 > 既存儀式的ルール` とする。
- `RULE-INDEX-IMPL-002` 実装系意図（追加/修正/バグ修正/リファクタ/機能追加/削除/ビルド/テスト/コミット）が含まれる依頼は、Plan提示で停止せず実装まで進める。
- `RULE-INDEX-IMPL-003` 相談系意図（案出し/比較/調査/説明のみ）が主目的の依頼のみ、Plan中心で返す。
- `RULE-INDEX-IMPL-004` 即時ブロッカーでない不確定事項は、仮定を明示して先に実装と検証を進める。
- `RULE-INDEX-IMPL-005` 実装依頼は `1リクエスト=1コミット` を既定とし、ユーザーが `コミットしない` と明示した場合のみコミットを省略する。
- `RULE-INDEX-IMPL-006` 既定実行モードは `Ultra Fast Path` とし、詳細手順は `AGENTS/20_development_rules.md` を唯一の正として適用する。
- `RULE-INDEX-IMPL-007` 既存ルールと本変更が競合する場合は、本変更（Ultra Fast Path方針）を優先する。

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
