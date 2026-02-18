# AGENTS Split: Project Rules (Rust + Bevy)

## Requirement Spec File Convention
要件定義書は次を唯一の正式ファイルとする。
- ファイル名: `要件定義_プロジェクト名.md`
- 配置: 各プロジェクトのトップディレクトリ
- `プロジェクト名` の決定者: Codex

運用ルール:
- Codex は初回サイクル開始時に `プロジェクト名` を決定し、報告で明示する。
- `プロジェクト名` の既定決定方法は「プロジェクトトップのディレクトリ名」を採用する。
- 決定後の `プロジェクト名` は固定し、変更時は `IMMUTABLE_CHANGE_REQUEST` を必須とする。
- Codex は各サイクル開始時に、解決済みの実ファイル名（例: `要件定義_bevy_game1.md`）を報告する。
- 以降「要件定義書」と書く場合は、常にこのファイルを指す。
- 要件定義書として扱う全ファイル（正式/旧形式を含む）の1行目には、対象ソフトの概要を20文字以内で記載する。
- 1行目より前にタイトル・見出し・空行を置かない。

## Project Identity (Rust Project Marker)
Rustプロジェクトとしての唯一の判定は「プロジェクトルートに `Cargo.toml` が存在すること」とする。
- `RULE-PROJ-RUST-001` `Cargo.toml` が無いディレクトリを Rust プロジェクトとして扱わない。
- `RULE-PROJ-RUST-002` `target/` は生成物であり、成果物・仕様・実装の入力源として扱わない。

## Bevy Project Marker (Optional)
Bevyプロジェクトかどうかの判定は `Cargo.toml` の依存に `bevy` が含まれるかで行う。
- `RULE-PROJ-BEVY-001` `Cargo.toml` を読み、`[dependencies]`（または workspace 依存）に `bevy` がある場合のみ「Bevyプロジェクト」と扱う。
- `RULE-PROJ-BEVY-002` Bevyプロジェクトで dev 実行が極端に遅い場合、次の profile を `Cargo.toml` に提案してよい（適用はユーザー指示がある場合のみ）。
  - `[profile.dev] opt-level = 1`
  - `[profile.dev.package."*"] opt-level = 3`

## Activation
実行時ルールの唯一の正（Single Source of Truth）は `AGENTS.md`（インデックス）と、そこで列挙された分割ファイル群とする。
同名・類似名の下書き/履歴ファイルは参照不要とし、実行時判断に使わない。

運用ルール:
- Codex は `AGENTS.md` に定義された読み込み順に従って分割ファイルを実行時ルールとして適用する。
- 他ドキュメントは参考情報として参照できるが、実行ルールとして扱うには分割ファイルへ明示的に転記されていることを要件とする。
- ルール変更時は対象分割ファイルを更新し、同一ルールを複数ファイルへ重複記載しない。

## Target Project Resolution
ワークスペース内に複数プロジェクトがある場合、毎サイクルで `target_project_root` を確定する。

決定順序:
1. ユーザーが明示したパス / プロジェクト名
2. 直前サイクルで使用した `target_project_root`
3. それでも曖昧なら、Codex が一度だけユーザーへ確認する
4. 3で回答が得られない場合は、ワークスペースルート `C:\Users\gonec\RustProjects` を既定の `target_project_root` として採用する

運用ルール:
- 以降の成果物（要件定義書、result.json、replay、golden）は `target_project_root` 配下に置く。
- `target_project_root` 未確定のまま実装を開始しない。

## Planning-First Policy
無駄な実装を避けるため、既定動作は「要件検討（Plan-First）」とする。

運用ルール:
- `RULE-PROJ-PLAN-001` Codex は既定で、要件整理・論点分解・テスト観点設計までを行い、実装/ビルド/副作用実行は行わない。
- `RULE-PROJ-PLAN-002` 実装モードへ移行する条件は、ユーザー入力に実行開始を示す明示キーワードが含まれる場合のみとする。
- `RULE-PROJ-PLAN-003` 明示キーワード例: `実装して` `修正して` `変更して` `追加して` `作って` `削除して` `コードを書いて` `ビルドして` `buildして` `exeを作って` `cargo run` `cargo test`
- `RULE-PROJ-PLAN-004` 明示キーワードがない依頼では、コード変更・ファイル更新・ビルド実行・副作用のあるコマンド実行を行わない（ただし `Mandatory Development Loop` の Step 8 に定義された発話用JSON出力は除く）。
- `RULE-PROJ-PLAN-005` 依頼が曖昧な場合は、実装せずに要件確認を優先する。

## Bootstrap And Migration
初回導入または既存資産がある場合の移行ルールを定義する。

初期化:
- `target_project_root` に `要件定義_プロジェクト名.md` が存在しない場合、Codex が作成する。
- 最低セクション:
  - `# 要件定義`
  - `# 詳細設計`
  - `# 手動テスト結果`
  - `# 変更履歴`

既存ファイル移行:
- 旧要件文書がある場合、初回のみ内容を正式ファイルへ統合する。
- 統合後、旧ファイルには「移行先ファイル名」を記載して凍結（追記禁止）する。
- 移行作業は `変更履歴` に記録する。
