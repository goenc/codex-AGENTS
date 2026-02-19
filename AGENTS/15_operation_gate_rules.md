# AGENTS Split: Operation Gate Rules

## Interaction-Minimization Priority
やり取り回数を減らして実装を完走しやすくする優先規則を定義する。

- `RULE-OG-IMPL-001` 本ファイルは `AGENTS.md` の `RULE-INDEX-IMPL-001..007` を常に参照し、運用判断へ適用する。
- `RULE-OG-IMPL-002` ルール競合時の判定基準は `AGENTS.md` の `RULE-INDEX-IMPL-001` を唯一の正として参照する。
- `RULE-OG-IMPL-003` 即時ブロッカーでない不確定事項は、質問で停止せず仮定を明示して実装を先行する。
- `RULE-OG-IMPL-004` 追加質問は「破壊的変更」「不可逆操作」「機密情報変更」「対象プロジェクトが一意に決まらない」のみ許可する。
- `RULE-OG-IMPL-005` ルール競合時の判定は `AGENTS.md` の `RULE-INDEX-IMPL-001` に従う。
- `RULE-OG-IMPL-006` 実装系依頼ではPlanのみ提示して停止することを禁止する。必ず最小実装 -> 検証 -> コミットまで進める。
- `RULE-OG-IMPL-007` 即時ブロッカーでない不足情報は、質問せずに仮定を明示して実装する。
- `RULE-OG-IMPL-008` 小規模実装において確認質問を禁止する。仮定を明示して実装を先行する。

## Startup Context Rules
開発再開時に必要なコンテキスト確認と、実装/相談モード判定を固定する。

- `RULE-PROJ-CONTEXT-001` `要件定義_プロジェクト名.md` には `# 要件定義` 直下で `target_project_root: <absolute_path>` を必須記載する。
- `RULE-PROJ-CONTEXT-002` `target_project_root` は当該サイクルで確定した `target_project_root` と完全一致させる。変更時は `# 変更履歴` に記録する。
- `RULE-PROJ-CONTEXT-003` 要件定義書の新規作成時は初版から `target_project_root` を記載し、空値を禁止する。
- `RULE-PROJ-CONTEXT-004` 開発再開ターンの開始時に、Codex は要件定義書の `target_project_root` 記載有無と値一致を確認し、不一致時は実装前に修正する。
- `RULE-PROJ-PLAN-001` 依頼が実装系意図を含む場合、Codex は Plan-only で停止せず `AGENTS/20_development_rules.md` の実装フローへ直接進む。
- `RULE-PROJ-PLAN-002` 依頼が相談系意図のみ（案出し/比較/調査/説明）で構成される場合のみ、Plan中心で返し実装は行わない。
- `RULE-PROJ-PLAN-003` 実装系意図の例: `実装` `修正` `変更` `追加` `作成` `削除` `バグ修正` `リファクタ` `コード` `build` `cargo` `test` `commit`
- `RULE-PROJ-PLAN-004` 相談系意図の例: `案を出して` `比較して` `調査して` `設計だけ` `実装しないで` `方針だけ`
- `RULE-PROJ-PLAN-005` 実装可能だが情報不足がある場合は、停止して質問せず仮定を明示して進める。
- `RULE-PROJ-PLAN-006` 破壊的変更（広範囲削除、不可逆マイグレーション、秘密情報更新）は1回だけ確認してから実行する。
- `RULE-PROJ-PLAN-007` 実装依頼では `1リクエスト=1コミット` を既定とし、ユーザーが `コミットしない` と明示した場合のみコミットを省略する。

## Speech Output Rules
開始時/終了時の発話用JSON出力に関するルールを定義する。

- `RULE-SPEECH-001` readaloud JSON は既定で無効とする。
- `RULE-SPEECH-002` ユーザーが明示要求した場合のみ readaloud JSON を生成する。
- `RULE-SPEECH-003` 生成時の出力先は常に `C:\Users\gonec\RustProjects\event.json` に固定する。
- `RULE-SPEECH-004` readaloud JSON の有無は実装フローの進行条件にしない。

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
- `RULE-DEV-WORKLOG-002` `CODEX_WORKLOG` は既定で完全無効とする。
- `RULE-DEV-WORKLOG-003` `CODEX_WORKLOG` は `500行以上の変更` または `依存更新（Cargo.toml/Cargo.lock変更）` の場合のみ有効化する。小変更では生成を禁止する。
- `RULE-DEV-WORKLOG-004` `RULE-DEV-WORKLOG-003` で有効化された場合に限り、`RULE-DEV-WORKLOG-001` のファイルが存在しなければテンプレート付きで新規作成する。
- `RULE-DEV-WORKLOG-005` ユーザーが編集中ログの表示を要求した場合、`CODEX_WORKLOG.md` の現内容を提示してよい。
- `RULE-DEV-WORKLOG-006` コミットメッセージ作成時は、必ず `git diff --staged` を根拠にし、worklog有効時のみ `CODEX_WORKLOG.md` と突合する。
- `RULE-DEV-WORKLOG-007` staged差分にAGENTS系ファイルとソフト実装系ファイルが混在する場合は、コミットを分割して個別メッセージを作成する（ユーザー明示許可がある場合のみ混在コミット可）。
- `RULE-DEV-WORKLOG-008` コミットメッセージのタイトルと本文は、`RULE-DEV-WORKLOG-006` の突合結果を根拠に生成する。
- `RULE-DEV-WORKLOG-009` コミット成功後、worklog有効時のみ `CODEX_WORKLOG.md` をテンプレート状態へリセットし、前コミット分のエントリを残さない。
- `RULE-DEV-WORKLOG-010` コミット失敗時、worklog有効時のみ `CODEX_WORKLOG.md` をリセットせず、失敗理由を追記して再試行に備える。
- `RULE-DEV-WORKLOG-011` 変更分類は `AGENTS.md` または `AGENTS/` 配下を `AGENT`、それ以外の実装/設定/テスト変更を `SOFT` とする。
- `RULE-DEV-WORKLOG-012` `CODEX_WORKLOG.md` の新規作成またはリセット時は、`# CODEX Worklog` `## Current Cycle` `## Entries` の3見出しを必須とする。
- `RULE-DEV-WORKLOG-013` コミット実行直前に `Commit Preflight` を必ず実施し、常時簡略版を適用する。
- `RULE-DEV-WORKLOG-014` `Commit Preflight` の必須チェックは `git diff --staged 確認` `コミットメッセージ形式適合` `実行した検証モード（Light/Full）の記録` の3点とする。
- `RULE-DEV-WORKLOG-015` `RULE-DEV-WORKLOG-014` のいずれかが `FAIL` の場合は `git commit` を実行してはならない。

## Result Artifact Rules
実行結果ファイルの最小出力要件を固定する。

- `RULE-DEV-RESULT-001` `target_project_root/result.json` には `result_mode: "minimal"` を必須出力する。
- `RULE-DEV-RESULT-002` `minimal` の必須項目は `result_mode` `os` `rustc_version` `cargo_version` `timestamp_utc` `config_policy_validation` とする。
- `RULE-DEV-RESULT-003` `cpu_arch` `target_triple` `locale` `timezone` `dependency_set` などの追加情報は、ユーザー明示要求または失敗再現性確保が必要な場合のみ任意で追記してよい。
- `RULE-DEV-RESULT-004` `RULE-DEV-RESULT-003` の追加情報は互換性確保のため `extra` オブジェクト配下に格納する。
- `RULE-DEV-RESULT-005` `config_policy_validation` が `FAIL` の場合は `config_policy_violations`（非空配列）を必須出力する。

## Output Coupling Rule
通常出力制約と発話ルールの境界条件を固定する。

- `RULE-OUT-COST-003` 読み上げJSONは既定で必須にせず、ユーザー明示要求時のみ本ファイルの最小出力制限の例外として扱う。
