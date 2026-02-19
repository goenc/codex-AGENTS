# AGENTS Split: Development Rules (Rust + Bevy)

## Interaction-Minimization Priority
やり取り回数を減らし、1回の指示で完走するための優先規則を定義する。

- `RULE-DEV-IMPL-001` 本ファイルは `AGENTS.md` の `RULE-INDEX-IMPL-001..007` を常に参照し、実装判断へ適用する。
- `RULE-DEV-IMPL-002` 既定実行モードは `Ultra Fast Path` とし、`Fast Path` を常時適用して Plan提示だけで停止しない。
- `RULE-DEV-IMPL-003` `Full Path` は `RULE-DEV-VERIFY-003` の条件に一致した場合のみ適用する。
- `RULE-DEV-IMPL-004` 不確定事項は、即時ブロッカーでない限り仮定を明示して先に実装と検証を進める。
- `RULE-DEV-IMPL-005` 実装可能な依頼でPlan-only停止を禁止する。停止が許可されるのは安全確認が必要な場合のみ。

## Primary Workflow (Execute-First Loop)
実装モードでは「Ultra Fast Path（最小実装 -> Light Verify -> 1コミット）」を既定の反復ループとする。

1. ユーザーが実装系依頼を出す
2. Codexが最小差分を実装し、Light VerifyまたはFull Verifyを実行する
3. Codexが1コミットで完了させる（`コミットしない` 指示時は除く）
4. 必要時のみ手動テストや追加設計へ進む

重要ルール:
- 小変更では工程儀式より完走を優先する。
- 安全性を損なう操作（破壊的変更）は確認を優先する。

## Target File Extraction Rules (Resume Recognition)
開発再開ターンで列挙対象を決定する固定ルールを定義する。

- `RULE-DEV-TARGETFILES-001` 目的: `target_project_root` 配下の「開発再開で列挙すべき対象ファイル」を決めるため、本節を固定ルールとして適用する。
- `RULE-DEV-TARGETFILES-002` 列挙対象の既定は `target_project_root` 配下の開発関連テキストファイルとし、`RULE-DEV-TARGETFILES-004` の強制除外を常に優先する。
- `RULE-DEV-TARGETFILES-003` Codexが列挙する場合の出力は、ファイルパスのみ、1行=1ファイル、辞書順（パス順）とし、説明文を付けない。
- `RULE-DEV-TARGETFILES-004` 強制除外はフォルダ `.git/` `target/` と、秘密情報 `.env` `*.key` `*.pem` `*token*` `*secret*` `*credential*` とする。
- `RULE-DEV-TARGETFILES-005` 列挙結果の上限は `300` ファイルまでとする。上限超過時は `RULE-DEV-TARGETFILES-006` の優先順で先頭から採用する。
- `RULE-DEV-TARGETFILES-006` 上限超過時の採用優先順は `Cargo.toml` `Cargo.lock` `要件定義_*.md` `src/**/*.rs` `tests/**/*.rs` `assets/config/**/*.json` `*.toml` `*.md` とする。

## Operation Gate Delegation
発話出力とコミット運用の固定ゲートは、`AGENTS/15_operation_gate_rules.md` の次のルールを唯一の正として適用する。

- `RULE-SPEECH-001..004`
- `RULE-DEV-COMMIT-001..008`
- `RULE-DEV-WORKLOG-001..015`

## Verification Modes (Two-Stage)
検証は二段階運用とし、既定は `Light Verify` とする。

- `RULE-DEV-VERIFY-001` `Light Verify`（既定）は `cargo fmt --all` + `対象テスト` または `cargo test` を実行し、`clippy` は任意とする。
- `RULE-DEV-VERIFY-002` `Full Verify` は `cargo fmt --all` `cargo clippy --all-targets --all-features -- -D warnings` `cargo test` `cargo build` の順で実行する。
- `RULE-DEV-VERIFY-003` `Full Verify` は次のいずれかの場合のみ実行する: `Cargo.toml` 変更 `Cargo.lock` 変更 `依存追加/更新` `500行以上の差分` `assets/fonts/` 配下の差分 `assets/config/*.default.json` の差分。
- `RULE-DEV-VERIFY-004` `Light Verify` では `cargo build` を省略してよいが、実行可能成果物が必要な依頼では `cargo build` を追加する。
- `RULE-DEV-VERIFY-005` 最終報告では、実行した検証モード（Light/Full）と実行コマンドを1-2行で記録する。
- `RULE-DEV-VERIFY-006` `RULE-DEV-VERIFY-003` の条件に一致しない場合、`Full Verify` の実行を禁止する。

## External Config Sync Rules
外部設定ファイルの同期・反映に関する固定ルールを定義する。

- `RULE-DEV-CONFIG-001` 同期元は `target_project_root/assets/config/*.default.json` のみとする。
- `RULE-DEV-CONFIG-002` 同期先は `target_project_root/target/debug/assets/config/` と `target_project_root/target/release/assets/config/` の2箇所とし、存在しない場合は作成してよい。
- `RULE-DEV-CONFIG-003` 同期操作は常にコピーで行い、同期元ファイルの移動・削除を禁止する。
- `RULE-DEV-CONFIG-004` 同期対象は `*.default.json` のみとし、`*.override.json` と `%AppData%` 配下の実行時データをプロジェクト原本へ自動コピーしない。
- `RULE-DEV-CONFIG-005` ユーザーが反映OKを明示した場合のみ、開発モードで `%AppData%\\<AppName>\\*.override.json` から `assets/config/*.default.json` へ反映してよい。
- `RULE-DEV-CONFIG-006` 設定同期を行った場合のみ、検証または最終報告で同期元と同期先2箇所の成否を短く記録する。
- `RULE-DEV-CONFIG-007` 外部設定を伴う実装では、要件整理時に `config_contract` を1行記録する。既定値は `default=assets/config/*.default.json` `runtime=%AppData%\\<AppName>\\*.override.json` とする。
- `RULE-DEV-CONFIG-008` 設定形式の静的検査は、`assets/config/*.default.json` に差分がある場合のみ実行し、対象ファイルの存在と非JSON設定参照を機械検出する。
- `RULE-DEV-CONFIG-009` `RULE-DEV-CONFIG-008` で違反が出た場合は実装完了判定を不可とし、修正完了までコミットしてはならない。
- `RULE-DEV-CONFIG-010` 設定検証を実施した場合は、最終報告に `config_policy_validation`（`PASS` / `FAIL`）を必ず記録する。
- `RULE-DEV-CONFIG-011` 設定ファイルの拡張子・相対パスは単一定義（共有定数または共通ローダー）で管理し、他箇所での拡張子文字列直書きを禁止する。

## Knowledge Update Rules
共通ナレッジの更新条件と、要件定義への反映条件を固定する。

- `RULE-DEV-KB-001` 再利用可能な原因/対処を特定した場合のみ、`User Manual Gate` で `OK` 後に `C:\Users\gonec\RustProjects\KNOWLEDGE.md` へ追記してよい。
- `RULE-DEV-KB-002` `KNOWLEDGE.md` の更新は追記方式（add-only）を既定とし、既存 `kb_id` の意味変更や削除はユーザー明示指示がある場合のみ許可する。
- `RULE-DEV-KB-003` 要件定義書を更新する場合は、`target_tags` に一致する `knowledge_refs` を同期する。

## Commit Intent Routing Rules
コミット指示時のコミット先を、曖昧入力と誤字を許容して決定する。

- `RULE-DEV-COMMIT-ROUTE-001` コミット先判定は入力正規化（NFKC、英字小文字化、空白と主要記号除去）後に行う。
- `RULE-DEV-COMMIT-ROUTE-002` `コミット意図` は、正規化後文字列に `コミット` `こみっと` `commit` `gitcommit` が含まれる場合、または `コミット` / `commit` への編集距離が1以内の場合に成立とする。
- `RULE-DEV-COMMIT-ROUTE-003` `エージェント系ヒント` は、正規化後文字列に `エージェント` `えーじぇんと` `エイジェント` `agent` `agents` `agnts` `agentsmd` `ルール` が含まれる場合、または `エージェント` / `agents` への編集距離が1以内の場合に成立とする。
- `RULE-DEV-COMMIT-ROUTE-004` `コミット意図` と `エージェント系ヒント` が同時成立した場合は `C:\Users\gonec\RustProjects`（AGENTS管理リポジトリ）へコミットする。
- `RULE-DEV-COMMIT-ROUTE-005` `コミット意図` が成立し `RULE-DEV-COMMIT-ROUTE-004` が不成立の場合は、`target_project_root`（プロジェクトリポジトリ）へコミットする。
- `RULE-DEV-COMMIT-ROUTE-006` コミット先が一意に決まらない場合のみ、1回だけ確認質問を行う。確認回答後は同一ターン内で再質問しない。
- `RULE-DEV-COMMIT-ROUTE-007` 明示指定（例: パス指定、`AGENTS` 指定、プロジェクト名指定）がある場合は `RULE-DEV-COMMIT-ROUTE-004..006` より優先する。

## Mandatory Development Loop (Ultra Fast First Order)
次の順序を固定し、既定は `Ultra Fast Path` とする。

### Ultra Fast Path (Default)
0. Intake
- 目的と完了条件を1文で定義する。
- `target_project_root` を確定する。
- 明示変更がない限り、直前サイクルの `target_project_root` を固定使用する。
- 破壊的変更の有無だけ先に判定し、該当時のみ確認する。

1. Delta Requirements
- 変更要求を最小単位で定義する。
- 即時ブロッカーでない不足情報は仮定として明記する。
- 外部設定を扱う場合のみ `RULE-DEV-CONFIG-007` の `config_contract` を記録する。

2. Minimal Implementation
- 最小差分のみ実装する。

3. Light Verify
- `RULE-DEV-VERIFY-001` に従って検証する。
- `RULE-DEV-VERIFY-003` に該当した場合のみ `Full Path` へ切り替える。

4. Commit & Handoff
- `RULE-DEV-WORKLOG-013..015` の Preflight を実施する。
- `1リクエスト=1コミット` を既定としてコミットする（ユーザーが `コミットしない` と明示した場合を除く）。
- 変更点/検証結果/残課題を短く報告する。

### Full Path (Conditional)
0. Intake
- Fast Path の0を実施する。
- Rustプロジェクト判定として `Cargo.toml` の存在を確認する（無い場合は実装に進まない）。

1. Delta Requirements
- 変更要求を `req_id` 単位で定義する。
- 影響I/O（保存データ、入力、設定、描画、時間依存）を記録する。
- 必要な手動確認観点を1つ以上含める。

2. Test Design First
- `req_id` ごとに `test_id` を作成し、1対1で対応させる。

3. Minimal Implementation
- テストを満たす最小差分のみ実装する。

4. Codex Verification (Full)
- `RULE-DEV-VERIFY-002` を実行する。
- `RULE-DEV-BUILD-001` `RULE-DEV-BUILD-002` `RULE-DEV-FONT-001..006` `RULE-DEV-CONFIG-012` を適用する。

5. User Manual Gate
- GUIアプリまたはユーザー要求時のみ、手動確認手順を提示する（OK/NGを記録）。
- `NG` の場合は Delta 更新へ戻る。

6. Post-OK Design Update
- `要件定義_プロジェクト名.md` に、今回 `OK` になった挙動のみ追記する。
- `RULE-DEV-LOOP-009` の条件に該当する場合のみ、コミット前に Step 7 まで実施する。

7. Next Cycle Requirement Sync
- `RULE-DEV-LOOP-007` の条件に該当する場合のみ、次サイクル要求を更新する。
- 未確定なら `RULE-DEV-LOOP-008` に従って `未確定` と明示する。

8. Record & Handoff
- `RULE-DEV-RESULT-001..005` の要件に従って結果を記録する。

## Build / Font / Loop Detail Rules
Fast/Full Path で参照する補助ルールを定義する。

- `RULE-DEV-BUILD-001` `cargo build` は `Full Verify` 実行時、または実行可能成果物が必要な依頼で必須とする。
- `RULE-DEV-BUILD-002` `cargo build` を実行し、バイナリターゲットがある場合は実行可能ファイルのパス（例: `target\\debug\\<bin_name>.exe`）を報告する。
- `RULE-DEV-BUILD-003` `result.json` を出力する場合、バイナリターゲットがあれば `executable_path`（相対または絶対）を記録する。
- `RULE-DEV-FONT-001` フォントポリシー検証は `assets/fonts/` 配下に差分がある場合のみ必須とする。
- `RULE-DEV-FONT-002` 最低限の禁止検査対象は `C:\\Windows\\Fonts`（および `/Windows/Fonts`）の直参照、`.ttc` 参照、`TextFont` での `Handle::default()` 依存とする。
- `RULE-DEV-FONT-003` `RULE-DEV-FONT-002` の検査はローカル検証またはCIに組み込む。
- `RULE-DEV-FONT-004` フォント実在確認または禁止パターン検査で違反が出た場合は実装完了判定を不可とする。
- `RULE-DEV-FONT-005` `result.json` を出力する場合は `font_policy`（`required_font` `resolved_font_path` `validation`）を記録する。
- `RULE-DEV-FONT-006` UIコードのみの軽微変更では、`assets/fonts/` に差分がない限りフォント検証を実行しない。
- `RULE-DEV-CONFIG-012` 設定静的検査は `assets/config/*.default.json` に差分がある場合のみ実施し、実施時は結果（PASS/FAIL）を最終報告へ出力する。
- `RULE-DEV-LOOP-007` 軽微実装では要件定義書を更新しない。
- `RULE-DEV-LOOP-008` 要件定義書の更新は、挙動確定または仕様追加が発生した場合のみ実施する。
- `RULE-DEV-LOOP-009` Step 6/7 のコミット前完了義務は、要件定義書を本サイクルで変更した場合のみ発火する。
- `RULE-DEV-SAFETY-001` 削除系変更が10ファイル以上に及ぶ場合のみ確認を入れる。10ファイル未満の削除は確認なしで進める。
