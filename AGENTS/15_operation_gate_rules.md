# AGENTS Split: Operation Gate Rules

## Startup Gate
開発再開時は次の順序で起動チェックを実施する。

- `RULE-OG-STARTUP-001` 最初に `再開_<project>.md` を読み込む。
- `RULE-OG-STARTUP-002` `再開_<project>.md` の入力形式が崩れていても、キー値の自動整形を優先して続行する。
- `RULE-OG-STARTUP-003` `project_root` は絶対パス必須とし、空値や相対パスを禁止する。
- `RULE-OG-STARTUP-004` `project_root` 配下に `不変条件_<project>.md` が存在することを必須とする。
- `RULE-OG-STARTUP-005` Rustの場合は `project_root` 配下に `Cargo.toml` と `src/` が存在することを必須とする。
- `RULE-OG-STARTUP-006` Rust以外の場合は `entry_points` に列挙された入口の存在を必須とする。
- `RULE-OG-STARTUP-007` `ignore_initially` は初期探索の強制除外として必ず適用し、明示解除まで探索対象に含めない。
- `RULE-OG-STARTUP-008` `read_first` がある場合は初期読込順を固定し、未定義キーは `unknown` として保持のみ行う。
- `RULE-OG-STARTUP-009` `RULE-OG-STARTUP-003..006` のいずれかが未成立の場合、実装を開始せず不足項目と再実行方針のみ報告する。

## Intent Gate
- `RULE-OG-INTENT-001` 実装系意図を含む依頼は Plan-only で停止せず、`AGENTS/20_development_rules.md` の実装フローへ進む。
- `RULE-OG-INTENT-002` 相談系意図のみの依頼は Plan中心で返す。
