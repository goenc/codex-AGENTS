# AGENTS Split: Development Rules (Rust + Bevy)

## Primary Workflow (User-Codex Loop)
このループを最優先ワークフローとして固定する。
実行時の詳細手順と判定は `Mandatory Development Loop` を唯一の正とし、本節は概要説明として扱う。
本節は、実装モードへ移行した後の反復開発サイクルを対象とする。

1. ユーザーが要件定義を書く（Codexへ依頼）
2. Codexが実装し、自動テストを実行する
3. ユーザーが実際に起動して手動テストする
4. 手動テストが `OK` の場合:
   - Codexが `要件定義_プロジェクト名.md` に「詳細設計」を追記する
5. 詳細設計追記後:
   - ユーザーとCodexで、`要件定義_プロジェクト名.md` の「要件定義」セクションに次の希望実装を追記・更新する
6. Codexが次の実装とテストを行う
7. 3に戻って反復する

重要ルール:
- ユーザー手動テストが `OK` になる前に、詳細設計を確定しない。
- 手動テストが `NG` の場合は、詳細設計追記を行わず、要件Delta更新 -> 再実装に戻る。
- 詳細設計は「実際に動いた挙動」を後追いで構造化して記録する（推測で確定しない）。

## Target File Extraction Rules (Resume Recognition)
開発再開ターンで列挙対象を決定する固定ルールを定義する。

- `RULE-DEV-TARGETFILES-001` 目的: `target_project_root` 配下の「開発再開で列挙すべき対象ファイル」を決めるため、本節を固定ルールとして適用する。
- `RULE-DEV-TARGETFILES-002` 列挙対象の既定は `target_project_root` 配下の開発関連テキストファイルとし、`RULE-DEV-TARGETFILES-004` の強制除外を常に優先する。
- `RULE-DEV-TARGETFILES-003` Codexが列挙する場合の出力は、ファイルパスのみ、1行=1ファイル、辞書順（パス順）とし、説明文を付けない。
- `RULE-DEV-TARGETFILES-004` 強制除外はフォルダ `.git/` `target/` と、秘密情報 `.env` `*.key` `*.pem` `*token*` `*secret*` `*credential*` とする。
- `RULE-DEV-TARGETFILES-005` 列挙結果の上限は `300` ファイルまでとする。上限超過時は `RULE-DEV-TARGETFILES-006` の優先順で先頭から採用する。
- `RULE-DEV-TARGETFILES-006` 上限超過時の採用優先順は `Cargo.toml` `Cargo.lock` `要件定義_*.md` `src/**/*.rs` `tests/**/*.rs` `assets/config/**/*.json` `*.toml` `*.md` とする。

## Speech Output Rules
開始時/終了時の発話用JSON出力に関する固定ルールを定義する。

- `RULE-SPEECH-001` 開始時の readaloud JSON は `Mandatory Development Loop` の Step 0 で必ず出力する。
- `RULE-SPEECH-002` 終了時の readaloud JSON は `Mandatory Development Loop` の Step 8 で必ず出力する。
- `RULE-SPEECH-003` `RULE-SPEECH-001..002` は実装モード時のみ適用し、Plan-First（非実装モード）では適用しない。

## External Config Sync Rules
外部設定ファイルの同期・反映に関する固定ルールを定義する。

- `RULE-DEV-CONFIG-001` 同期元は `target_project_root/assets/config/*.default.json` のみとする。
- `RULE-DEV-CONFIG-002` 同期先は `target_project_root/target/debug/assets/config/` と `target_project_root/target/release/assets/config/` の2箇所とし、存在しない場合は作成してよい。
- `RULE-DEV-CONFIG-003` 同期操作は常にコピーで行い、同期元ファイルの移動・削除を禁止する。
- `RULE-DEV-CONFIG-004` 同期対象は `*.default.json` のみとし、`*.override.json` と `%AppData%` 配下の実行時データをプロジェクト原本へ自動コピーしない。
- `RULE-DEV-CONFIG-005` ユーザーが反映OKを明示した場合のみ、開発モードで `%AppData%\\<AppName>\\*.override.json` から `assets/config/*.default.json` へ反映してよい。
- `RULE-DEV-CONFIG-006` Step 4 または Step 8 の報告では、同期元パスと同期先2箇所の成否を短く記録する。
- `RULE-DEV-CONFIG-007` 外部設定を伴う実装では、Step 1 で `config_contract` を1行記録する。既定値は `default=assets/config/*.default.json` `runtime=%AppData%\\<AppName>\\*.override.json` とする。
- `RULE-DEV-CONFIG-008` Step 4 では設定形式の静的検査を必須実行し、最低限 `assets/config/*.default.json` の存在、`%AppData%\\<AppName>\\*.override.json` の運用契約適合、`assets/config/` または `config/` 配下の非JSON設定ファイル/参照を機械検出する。
- `RULE-DEV-CONFIG-009` `RULE-DEV-CONFIG-008` で違反（非JSON形式の設定ファイル/参照を含む）が出た場合は実装完了判定を不可とし、修正完了まで `User Manual Gate` へ進まない。
- `RULE-DEV-CONFIG-010` Step 8 と最終報告には `config_policy_validation`（`PASS` / `FAIL`）を必ず記録する。
- `RULE-DEV-CONFIG-011` 設定ファイルの拡張子・相対パスは単一定義（共有定数または共通ローダー）で管理し、他箇所での拡張子文字列直書きを禁止する。

## Knowledge Update Rules
共通ナレッジの更新条件と、要件定義への反映条件を固定する。

- `RULE-DEV-KB-001` 再利用可能な原因/対処を特定した場合のみ、`User Manual Gate` で `OK` 後に `C:\Users\gonec\RustProjects\KNOWLEDGE.md` へ追記してよい。
- `RULE-DEV-KB-002` `KNOWLEDGE.md` の更新は追記方式（add-only）を既定とし、既存 `kb_id` の意味変更や削除はユーザー明示指示がある場合のみ許可する。
- `RULE-DEV-KB-003` Step 6 または Step 7 で要件定義書を更新する際は、`target_tags` に一致する `knowledge_refs` を同期する。

## Commit Message Rules
コミットメッセージの言語と記載対象を固定する。

- `RULE-DEV-COMMIT-001` コミットメッセージは日本語で記述する。
- `RULE-DEV-COMMIT-002` 実装変更（例: `src/` `Cargo.toml` `Cargo.lock` `assets/` `tests/`）を含むコミットでは、要件定義書を更新した事実をメッセージへ記載しない。
- `RULE-DEV-COMMIT-003` `RULE-DEV-COMMIT-002` のコミットメッセージは、実装差分の要点のみを記載する。
- `RULE-DEV-COMMIT-004` 変更対象が要件定義書や運用文書のみのコミットでは、要件定義更新をメッセージへ記載してよい。
- `RULE-DEV-COMMIT-005` コミットメッセージの1行目はタイトル行のみとし、行頭に記号（例: `-` `・` `*`）を付けない。
- `RULE-DEV-COMMIT-006` 2行目以降に変更点を列挙する場合、各行の行頭は必ず `・` とする。
- `RULE-DEV-COMMIT-007` 2行目以降の列挙では、`-` `*` など `・` 以外の箇条書き記号を使用しない。

## Commit Intent Routing Rules
コミット指示時のコミット先を、曖昧入力と誤字を許容して決定する。

- `RULE-DEV-COMMIT-ROUTE-001` コミット先判定は入力正規化（NFKC、英字小文字化、空白と主要記号除去）後に行う。
- `RULE-DEV-COMMIT-ROUTE-002` `コミット意図` は、正規化後文字列に `コミット` `こみっと` `commit` `gitcommit` が含まれる場合、または `コミット` / `commit` への編集距離が1以内の場合に成立とする。
- `RULE-DEV-COMMIT-ROUTE-003` `エージェント系ヒント` は、正規化後文字列に `エージェント` `えーじぇんと` `エイジェント` `agent` `agents` `agnts` `agentsmd` `ルール` が含まれる場合、または `エージェント` / `agents` への編集距離が1以内の場合に成立とする。
- `RULE-DEV-COMMIT-ROUTE-004` `コミット意図` と `エージェント系ヒント` が同時成立した場合は `C:\Users\gonec\RustProjects`（AGENTS管理リポジトリ）へコミットする。
- `RULE-DEV-COMMIT-ROUTE-005` `コミット意図` が成立し `RULE-DEV-COMMIT-ROUTE-004` が不成立の場合は、`target_project_root`（プロジェクトリポジトリ）へコミットする。
- `RULE-DEV-COMMIT-ROUTE-006` コミット先が一意に決まらない場合のみ、1回だけ確認質問を行う。確認回答後は同一ターン内で再質問しない。
- `RULE-DEV-COMMIT-ROUTE-007` 明示指定（例: パス指定、`AGENTS` 指定、プロジェクト名指定）がある場合は `RULE-DEV-COMMIT-ROUTE-004..006` より優先する。

## Mandatory Development Loop (Fixed Order)
次の順序を固定し、飛ばさない。

### 0. Intake
- 目的と完了条件を1文で定義する。
- `target_project_root` を確定する。
- Rustプロジェクト判定として `Cargo.toml` の存在を確認する（無い場合は実装に進まない）。
- 要件定義書 `要件定義_プロジェクト名.md` の存在と最小セクションを確認する。
- この時点で「今回サイクルで触る要件」と「触らない要件」を明示する。
- ゲートテンプレートをこの時点で必ず1行記録する。
- 作業前出力（readaloud JSON）を必ず先に行う。

### 1. Delta Requirements
- 変更要求を `req_id` 単位で定義する。
- 影響I/O（保存データ、入力、設定、描画、時間依存）を必ず書く。
- ユーザー手動確認で使う確認観点（最小チェックリスト）を1つ以上含める。
- 外部設定を扱う `req_id` では `RULE-DEV-CONFIG-007` の `config_contract` を必ず併記する。

### 2. Test Design First
- `req_id` ごとに `test_id` を作成し、1対1で対応させる。

### 3. Minimal Implementation
- テストを満たす最小差分のみ実装する。

### 4. Codex Verification (Rust)
- 既定の検証コマンド順序:
  1. `cargo fmt --all`
  2. `cargo clippy --all-targets --all-features -- -D warnings`
  3. `cargo test`
  4. `cargo build`
  5. GUIスモーク（Bevy等）: `cargo run` を起動し、即時クラッシュなしを確認する（ユーザー指示がある場合のみ）。
- `RULE-DEV-BUILD-001` 実装サイクル完了時に手動テストへ引き渡すため、Step 4 では `cargo build` を必須実行し、`target/debug` 成果物を最新化する。
- `RULE-DEV-BUILD-002` バイナリターゲットがある場合、`cargo build` 後に実行可能ファイルのパス（Windows例: `target\\debug\\<bin_name>.exe`）を報告する。
- `RULE-DEV-FONT-001` Step 4 ではフォントポリシー検証を必須実行し、要件定義書で指定されたフォント（未指定時は `assets/fonts/NotoSansJP-Regular.ttf`）の実在を確認する。
- `RULE-DEV-FONT-002` Step 4 では静的検査で禁止パターンを機械検出する。最低限の検査対象は `C:\\Windows\\Fonts`（および `/Windows/Fonts`）の直参照、`.ttc` 参照、`TextFont` での `Handle::default()` 依存とする。
- `RULE-DEV-FONT-003` `RULE-DEV-FONT-002` の検査はローカル検証またはCIに必ず組み込み、手動確認前に実行する。
- `RULE-DEV-FONT-004` フォント実在確認または禁止パターン検査で違反が出た場合は実装完了判定を不可とし、違反箇所を修正するまで `User Manual Gate` へ進まない。
- `RULE-DEV-CONFIG-012` Step 4 では `RULE-DEV-CONFIG-008` の静的検査結果（PASS/FAIL）を必ず出力する。
- 失敗時は、エラー全文、原因候補、再実行方針のみを短く報告する。

### 5. User Manual Gate
- GUIアプリの場合: ユーザーが手動確認できる状態まで起動手順を提示する（OK/NGを記録）。
- `NG` の場合は詳細設計を更新せず、Delta更新へ戻る。
- `OK` の場合は Step 6 へ進む。

### 6. Post-OK Design Update
- `要件定義_プロジェクト名.md` に詳細設計を追記する。
- 詳細設計には、今回 `OK` になった挙動のみを書く。
- `RULE-DEV-LOOP-009` コミット実行前に Step 6（詳細設計）と Step 7（要件定義）の更新を完了しなければならない（未更新のままコミット禁止）。

### 7. Next Cycle Requirement Sync
- `RULE-DEV-LOOP-007` コミット実行前に、次サイクルの要求を `要件定義_プロジェクト名.md` の「要件定義」へ追記または更新する。
- `RULE-DEV-LOOP-008` 次サイクル要求が未確定の場合は「未確定」と明示してから Step 8 に進む。

### 8. Record & Handoff
- `RULE-DEV-RESULT-001` `target_project_root/result.json` には `result_mode: \"minimal\"` を必須出力する。
- `RULE-DEV-RESULT-002` `minimal` の必須項目は `result_mode` `os` `rustc_version` `cargo_version` `timestamp_utc` `config_policy_validation` とする。
- `RULE-DEV-RESULT-003` `cpu_arch` `target_triple` `locale` `timezone` `dependency_set` などの追加情報は、ユーザー明示要求または失敗再現性確保が必要な場合のみ任意で追記してよい。
- `RULE-DEV-RESULT-004` `RULE-DEV-RESULT-003` の追加情報は互換性確保のため `extra` オブジェクト配下に格納する。
- `RULE-DEV-RESULT-005` `config_policy_validation` が `FAIL` の場合は `config_policy_violations`（非空配列）を必須出力する。
- `RULE-DEV-BUILD-003` バイナリターゲットがある場合、`result.json` に `executable_path`（相対または絶対）を記録する。
- `RULE-DEV-FONT-005` `result.json` には `font_policy` を記録し、`required_font` `resolved_font_path` `validation`（PASS/FAIL）を含める。
- 発話用JSONの出力先は常に `C:\Users\gonec\RustProjects\event.json` に固定する。

## Priority Order
競合時は次の優先順で判断する。
1. 安全性と決定的実行制約
2. `AGENTS.md`（インデックス）と、そこから参照される分割ルールファイル
3. ローカル実装都合
