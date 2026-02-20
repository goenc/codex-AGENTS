# AGENTS.md (Index)

## Scope
このファイルは `C:\Users\gonec\RustProjects` とその配下に適用する。
より深い階層に別の `AGENTS.md` がある場合はそちらを優先する。

## Ultra Fast Path Policy
やり取り回数とベースコストを削減し、1回の指示で完走しやすくするための補助原則を定義する。

- `RULE-INDEX-IMPL-001` 優先順位は次の単一定義を唯一の正とする。
  1. 安全性（破壊的変更確認のみ）
  2. 実装完走（小さいreqは1req=1commit）
  3. やり取り回数削減
  4. 既存儀式的ルール
- `RULE-INDEX-IMPL-002` 実装系意図（追加/修正/バグ修正/リファクタ/機能追加/削除/ビルド/テスト/コミット）が含まれる依頼は、Plan提示で停止せず実装まで進める。
- `RULE-INDEX-IMPL-003` 相談系意図（案出し/比較/調査/説明のみ）が主目的の依頼のみ、Plan中心で返す。
- `RULE-INDEX-IMPL-004` 即時ブロッカーでない不確定事項は、仮定を明示して先に実装と検証を進める。
- `RULE-INDEX-IMPL-005` 小さいreqは `1req=1commit` を既定とし、大きいreqは複数reqへ分割して各reqを1commitで完了する。
- `RULE-INDEX-IMPL-006` 既定実行モードは `Ultra Fast Path` とし、詳細手順は `AGENTS/20_development_rules.md` を唯一の正として適用する。
- `RULE-INDEX-IMPL-007` 既存ルールと本変更が競合する場合は、本変更（Ultra Fast Path方針）を優先する。
- `RULE-INDEX-IMPL-008` 不明点は `unknown` と明示し、推測で埋めない。

Optimization target: minimize round trips, minimize diff size, preserve contracts.
All commits may be externally audited.

## Split Structure
この `AGENTS.md` はインデックス専用とし、実行ルール本体は以下の分割ファイルに定義する。

読み込み順（上から順に適用）:
1. `AGENTS/10_project_rules.md`
2. `AGENTS/15_operation_gate_rules.md`
3. `AGENTS/20_development_rules.md`
4. `AGENTS/30_output_rules.md`
5. `AGENTS/98_maintenance_rules.md`

補足:
- Windowsでのexeロック防止ビルド運用は `AGENTS/20_development_rules.md` の `Build Procedure (Windows EXE Lock Prevention)` を参照する。

変更履歴:
- `AGENTS/99_change_log.md`（記録専用。実行ルールとしては適用しない）

## Safe Operation Rules
- ルールの重複記載を禁止する（1つのルールは1ファイルにのみ記載）。
- 競合時の判定基準は `RULE-INDEX-IMPL-001`（AGENTS.md の優先順位単一定義）のみとする。
- 分割ファイルの参照に失敗した場合は作業を開始せず、失敗点と再実行方針のみ報告する。
- ルール追加・変更時は、該当分割ファイルを更新し、変更履歴は `AGENTS/99_change_log.md` に追記する。
