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
- `RULE-DEV-TARGETFILES-002` include の既定は、ソース `*.rs`、ドキュメント `*.md` `*.txt`、設定/データ `*.json` `*.yaml` `*.yml` `*.toml` `*.ini`、スクリプト `*.ps1` `*.bat` とする。
  - Rust必須: `Cargo.toml` `Cargo.lock`
  - 典型: `.cargo/config.toml`（存在する場合）
  - テスト: `tests/` 配下、または `src/` 内の `mod tests` を含む `*.rs`
  - exclude の既定は、フォルダ `.git/` `target/` `node_modules/` `dist/` `build/` `logs/` `runs/` `tmp/`、ファイル `*.log` `*.tmp`、バイナリ全般（`*.png` `*.jpg` `*.jpeg` `*.webp` `*.gif` `*.bmp` `*.ico` `*.mp3` `*.wav` `*.mp4` `*.mov` `*.avi` `*.zip` `*.7z` `*.rar` `*.exe` `*.dll` `*.pdb` `*.pdf` など）とする。秘密情報 `.env` `*.key` `*.pem` `*token*` `*secret*` `*credential*` は常に除外する。
- `RULE-DEV-TARGETFILES-003` Codexが列挙する場合の出力は、ファイルパスのみ、1行=1ファイル、辞書順（パス順）とし、説明文を付けない。
- `RULE-DEV-TARGETFILES-004` include/exclude 適用時に判断に迷う場合は include を既定とする。ただし、exclude ルールを優先する。

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

### 2. Test Design First
- `req_id` ごとに `test_id` を作成し、1対1で対応させる。

### 3. Minimal Implementation
- テストを満たす最小差分のみ実装する。

### 4. Codex Verification (Rust)
- 既定の検証コマンド順序:
  1. `cargo fmt --all`
  2. `cargo clippy --all-targets --all-features -- -D warnings`
  3. `cargo test`
  4. GUIスモーク（Bevy等）: `cargo run` を起動し、即時クラッシュなしを確認する（ユーザー指示がある場合のみ）。
- 失敗時は、エラー全文、原因候補、再実行方針のみを短く報告する。

### 5. User Manual Gate
- GUIアプリの場合: ユーザーが手動確認できる状態まで起動手順を提示する（OK/NGを記録）。
- `NG` の場合は詳細設計を更新せず、Delta更新へ戻る。
- `OK` の場合は Step 6 へ進む。

### 6. Post-OK Design Update
- `要件定義_プロジェクト名.md` に詳細設計を追記する。
- 詳細設計には、今回 `OK` になった挙動のみを書く。

### 8. Record & Handoff
- `target_project_root/result.json` に最低限以下を出力:
  - OS
  - CPU architecture
  - rustc version
  - cargo version
  - target triple（取得可能な範囲）
  - locale
  - timezone
  - 依存セット（`Cargo.lock` がある場合はその存在を記録）
- 発話用JSONの出力先は常に `C:\Users\gonec\RustProjects\event.json` に固定する。

## Priority Order
競合時は次の優先順で判断する。
1. 安全性と決定的実行制約
2. `AGENTS.md`（インデックス）と、そこから参照される分割ルールファイル
3. ローカル実装都合
