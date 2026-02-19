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

## Font Policy (Rust + Bevy Common Minimum)
日本語表示の再現性を確保するため、フォント運用を次の固定ルールで管理する。

- `RULE-PROJ-FONT-001` 使用フォントは再配布可能ライセンスのもののみ許可する。
- `RULE-PROJ-FONT-002` Windows標準フォント（`C:\Windows\Fonts` 配下）への直接参照を禁止する。
- `RULE-PROJ-FONT-003` フォントは必ず `assets/fonts/` 配下へ同梱する。
- `RULE-PROJ-FONT-004` 許可するフォント形式は `.ttf` または `.otf` のみとする。
- `RULE-PROJ-FONT-005` `.ttc` の利用を禁止する。
- `RULE-PROJ-FONT-006` Bevyのデフォルトフォント依存（フォント未指定や `Handle::default()` 依存）を禁止する。
- `RULE-PROJ-FONT-007` 要件定義書にフォント指定がある場合は、その指定を最優先で適用する。
- `RULE-PROJ-FONT-008` 要件定義書にフォント指定がない場合の既定フォントは `assets/fonts/NotoSansJP-Regular.ttf` とする。
- `RULE-PROJ-FONT-009` 実装完了判定前に、対象フォントファイルが `assets/fonts/` 配下に実在することを必須とする。

## Runtime Config Policy (External Tunables)
再ビルド回数を抑えつつ調整値の再現性を保つため、外部設定ファイル運用を次の固定ルールで管理する。

- `RULE-PROJ-CONFIG-001` 外部化する固定値の原本は `target_project_root/assets/config/` 配下に置き、`*.default.json` を唯一の正式デフォルトとして扱う。
- `RULE-PROJ-CONFIG-002` `target/debug` `target/release` `dist` など生成物・配布物側の設定ファイルは複製のみとし、原本として扱わない。
- `RULE-PROJ-CONFIG-003` 実行時に生成・更新する設定は `%AppData%\\<AppName>\\*.override.json` に保存し、プロジェクト原本と分離する。
- `RULE-PROJ-CONFIG-004` 読み込み優先順位は常に `override -> default` とし、`override` が存在しない場合のみ `default` を使う。
- `RULE-PROJ-CONFIG-005` `override` から原本 `default` への反映は開発モードの明示操作時のみ許可し、通常実行とリリース配布物では禁止する。
- `RULE-PROJ-CONFIG-006` 設定ファイルの「移動」を禁止し、同期は常にコピーで行う。
- `RULE-PROJ-CONFIG-007` 外部設定フォーマットは JSON のみを許可し、既定設定の入力源は `*.default.json` のみとする。
- `RULE-PROJ-CONFIG-008` `assets/config/` または `config/` 配下の `.txt` は設定ファイルとして扱ってはならない。
- `RULE-PROJ-CONFIG-009` JSON 以外の形式（例: `.txt` `.toml` `.yaml`）を外部設定として無効とする。

## Activation
実行時ルールの唯一の正（Single Source of Truth）は `AGENTS.md`（インデックス）と、そこで列挙された分割ファイル群とする。
同名・類似名の下書き/履歴ファイルは参照不要とし、実行時判断に使わない。

運用ルール:
- Codex は `AGENTS.md` に定義された読み込み順に従って分割ファイルを実行時ルールとして適用する。
- 他ドキュメントは参考情報として参照できるが、実行ルールとして扱うには分割ファイルへ明示的に転記されていることを要件とする。
- ルール変更時は対象分割ファイルを更新し、同一ルールを複数ファイルへ重複記載しない。

## Target Project Resolution
ワークスペース内に複数プロジェクトがある場合、`target_project_root` を簡略運用で確定する。

決定順序:
1. ユーザーが明示したパス / プロジェクト名
2. 明示変更がない限り、直前サイクルで使用した `target_project_root` を固定使用する
3. 直前サイクル値がない初回のみ、ワークスペースルート `C:\Users\gonec\RustProjects` を既定の `target_project_root` として採用する

運用ルール:
- `RULE-PROJ-TARGET-001` 明示変更がない限り、`target_project_root` の再確認質問を行わない。
- `RULE-PROJ-TARGET-002` ユーザーが `target_project_root` を明示変更した場合のみ、当サイクルで更新する。
- `RULE-PROJ-TARGET-003` `target_project_root` 変更時は、要件定義書 `# 変更履歴` に記録する。
- `RULE-PROJ-TARGET-004` 初回確定後は、以降のサイクルで同値を継続使用する。
- 以降の成果物（要件定義書、result.json、replay、golden）は `target_project_root` 配下に置く。
- `target_project_root` 未確定のまま実装を開始しない。

## Operation Gate Delegation
再開時コンテキスト確認と Plan-First 判定は、`AGENTS/15_operation_gate_rules.md` の次のルールを唯一の正として適用する。

- `RULE-PROJ-CONTEXT-001..004`
- `RULE-PROJ-PLAN-001..007`

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

## Requirement Determinism Baseline
新規要件定義の初版から、再現性を高めるための固定仕様を必須化する。

- `RULE-PROJ-SPECDET-001` `要件定義_プロジェクト名.md` を初期作成する際、`# 要件定義` 内に「再現性固定仕様」節を必ず作成する。
- `RULE-PROJ-SPECDET-002` GUIを含む要件では、固定値として最低限 `ウィンドウサイズ` `余白/間隔(px)` `主要色(RGBまたはHex)` `行/セルサイズ` を明記する。
- `RULE-PROJ-SPECDET-003` `RULE-PROJ-SPECDET-001` の節には、イベント順序（例: 入力 -> 状態更新 -> 描画 -> ポーリング）を順序付きで明記する。
- `RULE-PROJ-SPECDET-004` 外部設定(JSON)を扱う要件では、キー別の優先順位・正規化・欠落時フォールバックを明記する。
- `RULE-PROJ-SPECDET-005` 表示競合があり得る要件（例: 選択状態とHEAD状態）では、表示優先順位を高 -> 低の順で必ず明記する。
- `RULE-PROJ-SPECDET-006` `RULE-PROJ-SPECDET-001..005` が未確定の場合は `未確定` と明記し、次回更新で固定値へ置換する期限を `# 変更履歴` に記録する。

## Common Knowledge Policy
共通ナレッジを単一原本で管理し、要件定義作成時は対象技術に一致する項目のみ参照する。

- `RULE-PROJ-KB-001` 共通ナレッジ原本は `C:\Users\gonec\RustProjects\KNOWLEDGE.md` を唯一の正とする。
- `RULE-PROJ-KB-002` 要件定義書を新規作成または更新する際は、対象技術タグ集合 `target_tags`（例: `rust` `eframe` `egui` `windows`）を先に確定する。
- `RULE-PROJ-KB-003` 要件定義書へ反映するナレッジは、`KNOWLEDGE.md` の `tags` と `target_tags` が一致する項目のみ採用する。
- `RULE-PROJ-KB-004` 要件定義書には採用した `kb_id` 一覧を `knowledge_refs` として記録し、該当なしの場合は `knowledge_refs: []` を明記する。
- `RULE-PROJ-KB-005` `KNOWLEDGE.md` の各項目は最小メタデータ `kb_id` `tags` `env` `verified_at` `summary` `action` を必須とする。
