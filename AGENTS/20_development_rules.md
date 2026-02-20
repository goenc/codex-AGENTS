# AGENTS Split: Development Rules (Source Driven)

## Primary Workflow
既定フローは次の4段階とする。

1. 変更要求（req）
2. 最小差分で実装
3. ローカル検証（軽量）
4. コミット

- `RULE-DEV-FLOW-001` 実装依頼では上記4段階を一続きで実行し、Plan提示だけで停止しない。
- `RULE-DEV-FLOW-002` reqは最小差分単位で定義する。
- `RULE-DEV-FLOW-003` 小さいreqは `1req=1commit` を既定とする（ユーザーが `コミットしない` と明示した場合を除く）。
- `RULE-DEV-FLOW-004` 大きいreqは複数reqに分割し、各reqごとに1commitで処理する。
- `RULE-DEV-FLOW-005` 入力形式が崩れていても自動整形で続行する。
- `RULE-DEV-FLOW-006` 不明点は `unknown` と明示し、推測で埋めない。

## Source-Driven Determination
- `RULE-DEV-SOURCE-001` 仕様確定はソースコードを第一優先とする（Rustでは `Cargo.toml` + `src/`）。
- `RULE-DEV-SOURCE-002` `不変条件_<project>.md` に反する変更は実装しない。
- `RULE-DEV-SOURCE-003` `再開_<project>.md` は探索制御にのみ使用し、仕様そのものの根拠には使わない。

## Lightweight Verification
- `RULE-DEV-VERIFY-001` 既定検証は軽量検証とする。
- `RULE-DEV-VERIFY-002` Rustの軽量検証は `cargo fmt --all` と `cargo test`（または影響範囲テスト）を基本とする。
- `RULE-DEV-VERIFY-003` 非Rustは `entry_points` に対応する最小テスト/検証コマンドを実行する。
- `RULE-DEV-VERIFY-004` 検証失敗時はコミットしない。

## Contract Promotion
事故や再発防止が必要な場合のみ、不変条件への昇格を検討する。

- `RULE-DEV-PROMOTE-001` 昇格検討は、障害再発防止または破壊的回帰の予防が必要な場合に限定する。
- `RULE-DEV-PROMOTE-002` 昇格候補は次を全て満たすこと。
  - 失敗条件を観測可能に判定できる。
  - 実装詳細に依存しない。
  - 将来の改修でも維持すべき契約である。
  - `must` / `must not` の契約文で短く記述できる。
- `RULE-DEV-PROMOTE-003` `RULE-DEV-PROMOTE-002` を満たさない事項は不変条件へ昇格しない。
