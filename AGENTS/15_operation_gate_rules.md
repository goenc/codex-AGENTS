# AGENTS Split: Operation Gate Rules

## Startup Context Rules
開発再開時に必要なコンテキスト確認と、実装モード判定を固定する。

- `RULE-PROJ-CONTEXT-001` `要件定義_プロジェクト名.md` には `# 要件定義` 直下で `target_project_root: <absolute_path>` を必須記載する。
- `RULE-PROJ-CONTEXT-002` `target_project_root` は当該サイクルで確定した `target_project_root` と完全一致させる。変更時は `# 変更履歴` に記録する。
- `RULE-PROJ-CONTEXT-003` 要件定義書の新規作成時は初版から `target_project_root` を記載し、空値を禁止する。
- `RULE-PROJ-CONTEXT-004` 開発再開ターンの開始時に、Codex は要件定義書の `target_project_root` 記載有無と値一致を確認し、不一致時は実装前に修正する。
- `RULE-PROJ-PLAN-001` Codex は既定で、要件整理・論点分解・テスト観点設計までを行い、実装/ビルド/副作用実行は行わない。
- `RULE-PROJ-PLAN-002` 実装モードへ移行する条件は、ユーザー入力に実行開始を示す明示キーワードが含まれる場合のみとする。移行後の実行手順は `AGENTS/20_development_rules.md` の `Mandatory Development Loop` を唯一の正として適用する。
- `RULE-PROJ-PLAN-003` 明示キーワード例: `実装して` `修正して` `変更して` `追加して` `作って` `削除して` `コードを書いて` `ビルドして` `buildして` `exeを作って` `cargo run` `cargo test`
- `RULE-PROJ-PLAN-004` 明示キーワードがない依頼では、コード変更・ファイル更新・ビルド実行・副作用のあるコマンド実行を行わず、`Mandatory Development Loop` の Step 0-8 と `RULE-SPEECH-001..002` も適用しない。
- `RULE-PROJ-PLAN-005` 依頼が曖昧な場合は、実装せずに要件確認を優先する。

## Speech Output Rules
開始時/終了時の発話用JSON出力に関する固定ルールを定義する。

- `RULE-SPEECH-001` 開始時の readaloud JSON は `Mandatory Development Loop` の Step 0 で必ず出力する。
- `RULE-SPEECH-002` 終了時の readaloud JSON は `Mandatory Development Loop` の Step 8 で必ず出力する。
- `RULE-SPEECH-003` `RULE-SPEECH-001..002` は実装モード時のみ適用し、Plan-First（非実装モード）では適用しない。
- `RULE-SPEECH-004` 発話用JSONの出力先は常に `C:\Users\gonec\RustProjects\event.json` に固定する。

## Commit Message Rules
コミットメッセージの言語と記載対象を固定する。

- `RULE-DEV-COMMIT-001` コミットメッセージは日本語で記述する。
- `RULE-DEV-COMMIT-002` 実装変更（例: `src/` `Cargo.toml` `Cargo.lock` `assets/` `tests/`）を含むコミットでは、要件定義書を更新した事実をメッセージへ記載しない。
- `RULE-DEV-COMMIT-003` `RULE-DEV-COMMIT-002` のコミットメッセージは、実装差分の要点のみを記載する。
- `RULE-DEV-COMMIT-004` 変更対象が要件定義書や運用文書のみのコミットでは、要件定義更新をメッセージへ記載してよい。
- `RULE-DEV-COMMIT-005` コミットメッセージの1行目はタイトル行のみとし、行頭に記号（例: `-` `・` `*`）を付けない。
- `RULE-DEV-COMMIT-006` 2行目以降に変更点を列挙する場合、各行の行頭は必ず `・` とする。
- `RULE-DEV-COMMIT-007` 2行目以降の列挙では、`-` `*` など `・` 以外の箇条書き記号を使用しない。
- `RULE-DEV-COMMIT-008` コミットメッセージは `1行目タイトル` `2行目空行` `3行目以降本文` の3部構成を必須とし、本文は1行以上を必須とする。

## Commit Worklog Rules
コミット間の実装概要ログ運用と、コミットメッセージ生成時の根拠を固定する。

- `RULE-DEV-WORKLOG-001` コミット間ログの出力先は `C:\Users\gonec\RustProjects\CODEX_WORKLOG.md` に固定する。
- `RULE-DEV-WORKLOG-002` `RULE-DEV-WORKLOG-001` のファイルが存在しない場合は、コミット関連処理の前にテンプレート付きで新規作成する。
- `RULE-DEV-WORKLOG-003` 実装中の作業ログは Markdown の追記方式とし、時系列（古い順）を維持する。
- `RULE-DEV-WORKLOG-004` 各ログ行には最低限 `timestamp` `scope([AGENT]/[SOFT])` `summary` を含める。
- `RULE-DEV-WORKLOG-005` ユーザーが編集中ログの表示を要求した場合、`CODEX_WORKLOG.md` の現内容を提示してよい。
- `RULE-DEV-WORKLOG-006` コミットメッセージ作成時は、必ず `git diff --staged` と `CODEX_WORKLOG.md` を突合し、staged差分に存在する事実のみを採用する。
- `RULE-DEV-WORKLOG-007` staged差分にAGENTS系ファイルとソフト実装系ファイルが混在する場合は、コミットを分割して個別メッセージを作成する（ユーザー明示許可がある場合のみ混在コミット可）。
- `RULE-DEV-WORKLOG-008` コミットメッセージのタイトルと本文は、`RULE-DEV-WORKLOG-006` の突合結果を根拠に生成する。
- `RULE-DEV-WORKLOG-009` コミット成功後は `CODEX_WORKLOG.md` をテンプレート状態へリセットし、前コミット分のエントリを残さない。
- `RULE-DEV-WORKLOG-010` コミット失敗時は `CODEX_WORKLOG.md` をリセットせず、失敗理由を追記して再試行に備える。
- `RULE-DEV-WORKLOG-011` 変更分類は `AGENTS.md` または `AGENTS/` 配下を `AGENT`、それ以外の実装/設定/テスト変更を `SOFT` とする。
- `RULE-DEV-WORKLOG-012` `CODEX_WORKLOG.md` の新規作成またはリセット時は、`# CODEX Worklog` `## Current Cycle` `## Entries` の3見出しを必須とする。
- `RULE-DEV-WORKLOG-013` コミット実行直前に `Commit Preflight` を必ず実施し、`CODEX_WORKLOG.md` の最新行へ `preflight` 結果を追記する。
- `RULE-DEV-WORKLOG-014` `Commit Preflight` の必須チェックは `scope整合(AGENT/SOFT)` `git diff --staged 突合完了` `コミットメッセージ本文1行以上(行頭「・」)` の3点とする。
- `RULE-DEV-WORKLOG-015` `Commit Preflight` のいずれかが `FAIL` の場合、`git commit` を実行してはならない。

## Result Artifact Rules
実行結果ファイルの最小出力要件を固定する。

- `RULE-DEV-RESULT-001` `target_project_root/result.json` には `result_mode: "minimal"` を必須出力する。
- `RULE-DEV-RESULT-002` `minimal` の必須項目は `result_mode` `os` `rustc_version` `cargo_version` `timestamp_utc` `config_policy_validation` とする。
- `RULE-DEV-RESULT-003` `cpu_arch` `target_triple` `locale` `timezone` `dependency_set` などの追加情報は、ユーザー明示要求または失敗再現性確保が必要な場合のみ任意で追記してよい。
- `RULE-DEV-RESULT-004` `RULE-DEV-RESULT-003` の追加情報は互換性確保のため `extra` オブジェクト配下に格納する。
- `RULE-DEV-RESULT-005` `config_policy_validation` が `FAIL` の場合は `config_policy_violations`（非空配列）を必須出力する。

## Output Coupling Rule
通常出力制約と発話ルールの境界条件を固定する。

- `RULE-OUT-COST-003` 開始時/終了時の読み上げJSONは、`RULE-PROJ-PLAN-002` により実装モードへ移行している場合のみ必須とする。該当時は本ファイルの最小出力制限の例外として扱う。
