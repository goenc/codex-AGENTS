# AGENTS.md (Index)

## Scope
このファイルは `C:\Users\gonec\RustProjects` とその配下に適用する。
より深い階層に別の `AGENTS.md` がある場合はそちらを優先する。

## Goal
目的は「同人ソフトを、なるべく手戻りなく開発すること」。
実行可能な要件とテストを唯一の判定基準として扱い、実装は再生成可能な形で進める。

## Split Structure
この `AGENTS.md` はインデックス専用とし、実行ルール本体は以下の分割ファイルに定義する。

読み込み順（上から順に適用）:
1. `AGENTS/10_project_rules.md`
2. `AGENTS/20_development_rules.md`
3. `AGENTS/30_output_rules.md`

## Safe Operation Rules
- ルールの重複記載を禁止する（1つのルールは1ファイルにのみ記載）。
- 競合時は、後段ファイルより前段ファイルを優先するのではなく、`AGENTS/20_development_rules.md` の `Priority Order` を唯一の判定基準とする。
- 分割ファイルの参照に失敗した場合は作業を開始せず、失敗点と再実行方針のみ報告する。
- ルール追加・変更時は、インデックスではなく該当分割ファイルを更新する。

## AGENTS Update Hardening
`AGENTS.md` / `AGENTS/` 配下を更新する際は、以下を必須とする。

- `HARD-001` ルールID付与: 追加・変更するルールには一意ID（例: `RULE-OUT-001`）を付ける。
- `HARD-002` 2段階反映: 直接上書きせず、草案作成 -> 検証通過 -> 反映の順で更新する。
- `HARD-003` 事前検証: 参照先存在、見出し、重複ルール、優先順位矛盾を確認してから反映する。
- `HARD-004` 世代保存: 反映前にスナップショット（日時付き）を保存する。
- `HARD-005` 即時復旧: 直前世代へ戻す手順を常に保持し、失敗時は即復旧する。

更新時テンプレート（追記して使う）:
- `change_id`: CHG-YYYYMMDD-HHMM
- `target_files`: 変更対象ファイル一覧
- `rules_added_or_updated`: ルールID一覧
- `validation_result`: PASS / FAIL（理由）
- `rollback_point`: 復旧元スナップショット
- `notes`: 注意点

## Child Override Note
配下に独自 `AGENTS.md` がある場合、その配下では当該ファイルが優先される。
