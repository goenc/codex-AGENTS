# AGENTS Split: Project Rules (Source Driven)

## Core Artifacts
開発再開と実装判断の核は次の3点のみとする。

- `RULE-PROJ-ARTIFACT-001` 正式入力は `再開_<project>.md` `不変条件_<project>.md` `ソースコード（Rustでは Cargo.toml + src/）` のみとする。
- `RULE-PROJ-ARTIFACT-002` `不変条件_<project>.md` 先頭には `PROJECT_ID` と `PURPOSE` を1〜2行で必ず記載する。
- `RULE-PROJ-ARTIFACT-003` `再開_<project>.md` は探索制御専用とし、キーと値のみを許可する（説明文禁止）。
- `RULE-PROJ-ARTIFACT-004` `不変条件_<project>.md` は破壊禁止契約のみを許可する（UI仕様、実装方法、将来構想は禁止）。
- `RULE-PROJ-ARTIFACT-005` AGENTS群は共通母艦として運用し、プロジェクト固有仕様は保持しない。
- `RULE-PROJ-ARTIFACT-006` `再開_<project>.md` の探索制御キーは `project_root` `read_first` `ignore_initially` `ignore` `mode` `keywords` `entry_points` を基本とし、追加キーも説明文なしのキー値形式で扱う。

## Single Source of Truth
Single Source of Truth（SoT）を領域別に固定する。

- `RULE-PROJ-SOT-001` 挙動仕様の第一優先はソースコード（Rustでは `Cargo.toml` + `src/`）とする。
- `RULE-PROJ-SOT-002` 破壊禁止契約の第一優先は `不変条件_<project>.md` とする。
- `RULE-PROJ-SOT-003` 探索範囲固定の第一優先は `再開_<project>.md` とする。
- `RULE-PROJ-SOT-004` 矛盾時は領域責務で裁定する。仕様矛盾はソース修正を優先し、契約変更は明示要求がある場合のみ実施する。

## Entry Point Policy
- `RULE-PROJ-ENTRY-001` Rust判定は `project_root` 直下の `Cargo.toml` と `src/` の存在で行う。
- `RULE-PROJ-ENTRY-002` Rust以外の言語は `再開_<project>.md` の `entry_points` を唯一の入口定義として扱う。
- `RULE-PROJ-ENTRY-003` 生成物ディレクトリ（例: `target/`）を仕様確定の入力源として扱わない。
