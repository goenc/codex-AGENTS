# AGENTS Split: Development Rules

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

## Resume Recognition Mode
開発再開時の初動を、認識専用モードとして固定する。

- `RULE-DEV-RESUME-001` ユーザー入力に `開発再開` が含まれるターンでは、Codexは仕様書と必要ファイルの認識・列挙のみを行う。
- `RULE-DEV-RESUME-002` 上記ターンでは、実装・コード変更・テスト実行・ビルド・アプリ起動検証を禁止する。
- `RULE-DEV-RESUME-003` 認識結果は最低限、`target_project_root`、要件定義書、主要実装ファイル、主要テストファイル、設定ファイルを含めて報告する。
- `RULE-DEV-RESUME-004` 実装フェーズへ移るには、開発再開ターン完了後にユーザーから別ターンで実装指示を受けることを必須とする。
- `RULE-DEV-RESUME-005` 開発再開ターンでの主要実装ファイル・主要テストファイル・設定ファイルの列挙は `RULE-DEV-TARGETFILES-002..004` に従う。
- `RULE-DEV-RESUME-006` Resume Recognition Mode では、対象ファイル列挙の入力源を `target_project_root` 配下のファイルツリー（パス一覧）に固定する。ファイル内容の読み取りは原則行わず、必要な場合に限り最大 `N=6` ファイルまでとする。

## Target File Extraction Rules (Resume Recognition)
開発再開ターンで列挙対象を決定する固定ルールを定義する。

- `RULE-DEV-TARGETFILES-001` 目的: `target_project_root` 配下の「開発再開で列挙すべき対象ファイル」を決めるため、本節を固定ルールとして適用する。
- `RULE-DEV-TARGETFILES-002` include の既定は、ソース `*.py`、ドキュメント `*.md` `*.txt`、設定/データ `*.json` `*.yaml` `*.yml` `*.toml` `*.ini`、スクリプト `*.ps1` `*.bat`、テスト `tests/` 配下または `test_*.py` `*_test.py` とする。exclude の既定は、フォルダ `.git/` `__pycache__/` `.venv/` `venv/` `node_modules/` `dist/` `build/` `.mypy_cache/` `.pytest_cache/` `.ruff_cache/` `logs/` `runs/` `tmp/`、ファイル `*.log` `*.tmp`、バイナリ全般（`*.png` `*.jpg` `*.jpeg` `*.webp` `*.gif` `*.bmp` `*.ico` `*.mp3` `*.wav` `*.mp4` `*.mov` `*.avi` `*.zip` `*.7z` `*.rar` `*.exe` `*.dll` `*.pdf` など）とする。秘密情報 `.env` `*.key` `*.pem` `*token*` `*secret*` `*credential*` は常に除外する。
- `RULE-DEV-TARGETFILES-003` Codexが列挙する場合の出力は、ファイルパスのみ、1行=1ファイル、辞書順（パス順）とし、`explanations` `bullets` `commentary`（説明文・箇条書き・コメント）を付けない。
- `RULE-DEV-TARGETFILES-004` include/exclude 適用時に判断に迷う場合は include（対象に含める）を既定とする。ただし、exclude ルールを優先する。

## UI Placement Rules（Global）
全プロジェクト共通の子ダイアログ配置規約を定義する。

- `RULE-UI-DIALOG-001` 子ダイアログ（sub dialog / child window / modal）は、親ウィンドウの中心に配置する。子の中心 = 親の中心を既定とする。
- `RULE-UI-DIALOG-002` 配置計算は可能な限り親のクライアント領域基準で行う。クライアント領域が取得できない場合はウィンドウ矩形基準でもよいが、その場合は理由を `notes` に記録する。
- `RULE-UI-DIALOG-003` 例外としてオフセット指定を許可する。方式は `子の左上座標 = 親中心 - 子サイズ/2 + (dx, dy)` とし、`dx`, `dy` の既定は `0` とする。
- `RULE-UI-DIALOG-004` 画面外にはみ出す場合は、画面内に収まるように座標を clamp する（最小マージンを許可）。
- `RULE-UI-DIALOG-005` 実装時は各プロジェクトで同等のヘルパ関数を用意し再利用する（例: `center_dialog_on_parent(parent, child, dx=0, dy=0)`）。フレームワークごとの実装差は許容するが、入出力と挙動は本ルールに一致させる。
- `RULE-UI-DIALOG-006` 手動テスト観点（最小）は、親を画面内の任意位置へ移動した状態で子を開き子が親中心に出ること、`dx`,`dy` 指定時に指定分だけずれること、小さい画面/端寄せでも画面外に出ないこと（clamp）とする。

## Priority Order
競合時は次の優先順で判断する。
1. 安全性と決定的実行制約
2. `AGENTS.md`（インデックス）と、そこから参照される分割ルールファイル
3. `同人ソフト開発方針バイブコーディング.txt`（存在する場合の参考資料。矛盾時は 2 を優先）
4. ローカル実装都合

補足:
- `RULE-DEV-PRIORITY-001` 本ファイル内で競合する場合、限定条件が明示されたルールを優先し、同程度に具体的な場合は新しい `RULE-*` ID を優先する。

## Non-Negotiables
- フォルダ保存、PC操作、ファイル上書きの確認は不要（CLI全体）。
- 実装に着手したら、途中確認なしで動作テスト完了まで進める。
- ロールバック禁止。失敗は「要件Delta更新」または「明示的無効化」で解決する。
- コード検査は最小範囲に限定し、既存実装の把握は「変更対象と直接依存する範囲のみ」を許可する（不要な横断調査を禁止）。
- 同一 `test_id` が3回連続失敗した場合は、当該 `test_id` に限り最小トレース確認を許可する。
- 1つのテストは1つの要件のみを検証する（`test_id` と `req_id` を1対1で対応）。
- `expected` 更新は Delta要件で明示された場合のみ許可。
- `expected` を更新する場合は、誤動作を落とす break test を同時に追加する。
- 候補選定スコアは `public 80% : hidden replay 20%` を固定で使う。

## Lightweight Mode（軽微変更）
次を満たす変更は軽微変更として扱ってよい。
- UI文言修正、配置調整、小さなイベント配線変更
- 既存I/O契約を変更しない内部実装修正

軽微変更の最小要件:
- `req_id` は1件でよい（`REQ-LITE-xxx` 形式可）
- `test_id` は1件でよい（既存テスト再利用可）
- 変更範囲/非範囲/受け入れ条件は3行要約でよい

追加ルール:
- `RULE-DEV-LITE-001` Lightweight Mode は、UI文言/表示色/配置の非ロジック変更、ログ/通知/可視化のみの変更、既存処理の配線変更のみ、または変更対象が1-2ファイルのいずれかに該当する場合に自動提案し、既定適用してよい。
- `RULE-DEV-LITE-002` 以下を含む場合は Lightweight Mode を禁止する: 入力処理、ショートカットキー操作、並列処理、非同期処理、永続化（ファイル/DB/設定保存）、ネットワーク/外部通信、セキュリティ/権限/実行制御。
- `RULE-DEV-LITE-003` Lightweight Mode 適用時は、要件再定義と詳細テスト設計を省略してよい。
- `RULE-DEV-LITE-004` Lightweight Mode 適用時は、Step 8 の発話用JSONを `mini_event` 形式で出力してよい（条件は `Event Log Policy (Conditional Optimization)` に従う）。

## Global Optimization Policy
使用量削減を目的に、ターン数・探索量・出力量を抑える。

- `RULE-DEV-OPT-001` ターン数削減を最優先とし、1ターンで完結可能な作業は必ずまとめて実行する。
- `RULE-DEV-OPT-002` 不要な確認ターンと不要な中間報告を禁止する。
- `RULE-DEV-OPT-003` 実行前の探索は変更対象と直接依存範囲へ限定し、横断探索を行わない。
- `RULE-DEV-OPT-004` 出力は結果中心・説明最小を既定とする（詳細は `AGENTS/30_output_rules.md` を適用）。
- `RULE-DEV-OPT-005` 安全性・再現性・デバッグ性を損なう恐れがある場合に限り、`RULE-DEV-OPT-001..004` の抑制を例外的に緩和してよい。

## Reasoning Effort Auto Policy
モードごとに推論強度の既定値を定義する。

- `RULE-DEV-REASON-001` implement モードの既定は `reasoning_effort=medium` とする。
- `RULE-DEV-REASON-002` plan / review / diagnose / consult モードの既定は `reasoning_effort=low` とする。
- `RULE-DEV-REASON-003` 以下の場合のみ、既定値から1段階だけ引き上げてよい: 原因不明不具合の調査、仕様曖昧で破壊的変更の可能性がある場合、ユーザーが明示的に高精度を要求した場合。

## Event Log Policy (Conditional Optimization)
`event.json` は作業影響に応じて通常版と簡略版を使い分ける。

- `RULE-DEV-EVT-001` 通常版 `event.json` が必須な条件: ファイル変更が発生した場合、テスト/ビルド実行を行った場合、プロジェクト状態に影響する作業を行った場合。
- `RULE-DEV-EVT-002` `mini_event` を許可する条件: 相談/方針確認のみ、返答のみ（コード・設定変更なし）、軽微変更の提案/判断のみ。
- `RULE-DEV-EVT-003` `mini_event` の最低項目は `action_summary`（1行）と `affected_files`（空配列可）とする。
- `RULE-DEV-EVT-004` `before_work` の `payload` は簡略固定テンプレートとして `prompt_understanding`（1行）、`scope`（最大2項目）、`non_goals`（最大2項目）を必須とする。
- `RULE-DEV-EVT-005` Step 8 の出力先固定、言語制約、`before_work` 失敗時中断、`before_work` -> `after_work` の順序制約は維持する。
- `RULE-DEV-EVT-006` 本節の条件に該当する場合、Step 8 の「全応答で毎回通常版出力」は `mini_event` への置換を許可する。

## File Access & Search Policy
探索コスト削減のため、参照上限と探索前提を固定する。

- `RULE-DEV-SEARCH-001` 1ターンで参照してよいファイル数は原則最大6ファイルとする。
- `RULE-DEV-SEARCH-002` 以下の条件を満たす場合のみ、参照上限を最大12ファイルまで拡張してよい: 既存コード改修、原因不明バグ修正、依存関係が複数層に及ぶ場合、ユーザーが全体影響確認を要求した場合。
- `RULE-DEV-SEARCH-003` grep/ripgrep 等の探索前に、対象ファイルまたは対象関数範囲を明示する。
- `RULE-DEV-SEARCH-004` 「念のため」や「全体把握のみ」を目的とする横断探索を禁止する。

## Anti-Rework Principles
手戻り削減のため、以下を必須とする。
- 実装前に「今回変更の要件境界」を凍結する（Scope Freeze）。
- 仕様未確定のまま実装しない。未確定は `TBD` として明示し、影響がある場合は実装を止める。
- 変更対象外の機能に触れない（No Opportunistic Refactor）。
- I/O契約変更は実装より先に契約更新、または同一コミット内で同時更新する。
- 再現性のため、乱数シード・時間刻み・依存バージョンを固定可能にする。
- ユーザー手動テストの合否を、次工程へ進むゲートとして必ず記録する。

## Mandatory Development Loop (Fixed Order)
次の順序を固定し、飛ばさない。  
この順序は上記 `Primary Workflow` を実装レベルに分解したもの。

### 0. Intake
- 目的と完了条件を1文で定義する。
- `target_project_root` を確定する。
- 要件定義書 `要件定義_プロジェクト名.md` の存在と最小セクションを確認する。
- 参照順:
  1. `docs/spec/OVERVIEW.md`
  2. `docs/contracts/`
  3. 必要時のみ `docs/design/`
- 参照先が未作成の場合は、要件定義書を暫定の唯一参照元として先に進める（未作成を記録）。
- この時点で「今回サイクルで触る要件」と「触らない要件」を明示する。
- ユーザーが送ったプロンプト（コマンド形式を含む）をどう解釈したか、作業開始前に必ず「現在の読み上げ対象JSON」へ出力する。
- 上記の作業前出力は、実装・テスト・副作用のあるコマンド実行より先に行う。
- 作業前出力の最低項目は `type="readaloud"` `phase="before_work"` `payload.text` `payload.prompt_understanding` `payload.scope` `payload.non_goals` とする。
- 作業前出力に失敗した場合は作業を開始せず、失敗点と再実行方針のみ報告する。

### 1. Delta Requirements
- 変更要求を `req_id` 単位で定義する。最低項目:
  - `req_id`
  - 変更理由
  - 変更範囲 / 非範囲（non-goals）
  - 受け入れ条件（機械判定可能）
  - 影響I/O（保存データ、入力、設定、描画、時間依存）
- ユーザー手動確認で使う確認観点（最小チェックリスト）を1つ以上含める。

### 2. Test Design First
- `req_id` ごとに `test_id` を作成し、1対1で対応させる。
- 先に失敗するテストを用意し、意図しない成功を防ぐ。
- `expected` 変更がある場合:
  - Delta要件で明示
  - break test を同時追加

### 3. Minimal Implementation
- テストを満たす最小差分のみ実装する。
- 既存互換（特にセーブデータ）を暗黙に壊さない。
- 境界I/Oで例外を握りつぶさない。

### 4. Codex Verification
- publicテスト + replayテストを実行し、合否を記録する。
- GUIアプリの場合、Codexは実装後に必ず対象アプリを起動してスモーク確認する（起動成功、初期画面表示、即時クラッシュなし）。
- スモーク確認は「Codexが起動実行 -> 一定時間監視 -> 終了」を標準手順とし、結果を報告に含める。
- `RULE-DEV-ICON-001` Windows向けexeでアイコン（`.ico` または `spec` の `icon` 指定）を変更したサイクルでは、ビルド後に `RT_GROUP_ICON` / `RT_ICON` を機械検証し、少なくとも `16x16` `24x24` `32x32` の存在を確認して記録する。
- `RULE-DEV-ICON-002` 上記サイクルでは、生成exeの `32x32` 抽出アイコンが入力 `.ico` の `32x32` と一致することを機械検証して記録する。
- UI/操作感は「ユーザー手動確認」に切り分ける。
- 失敗時の扱い:
  - 1-2回目: 要件 or 実装の不整合を整理し、Delta更新で再実行
  - 3回目: 同一 `test_id` に限り最小トレース確認

### 5. User Manual Gate
- GUIアプリの場合: 実装完了後、Codexはユーザーの手動テスト用に対象アプリを起動した状態で引き渡す（ユーザー指示がない限りCodex側で終了しない）。
- `RULE-DEV-ICON-003` Windows向けexeのアイコン変更サイクルでは、Explorerキャッシュ影響が疑われる場合に限り `*_iconcheck_YYYYMMDD_HHMMSS.exe` の別名コピー作成を任意で実施できる。
- 非GUIの場合: ユーザーが同等の手動確認を実施できる最小コマンド/手順を提示して引き渡す。
- ユーザーが実機起動して確認し、結果を `OK/NG` で記録する。
- 記録形式は最低限次を含む:
  - 実施日（YYYY-MM-DD）
  - 実施環境（OS / バージョン）
  - 結果（OK/NG）
  - 観測内容（再現手順・症状・補足）
- `NG` の場合:
  - 詳細設計は更新しない
  - Delta要件を更新し、Step 2へ戻る
- `OK` の場合:
  - Step 6へ進む

### 6. Post-OK Design Update
- `要件定義_プロジェクト名.md` に詳細設計を追記する。
- 詳細設計には、今回 `OK` になった挙動のみを書く。
- 未実装や未検証の内容は詳細設計に混ぜない。

### 7. Next Requirement Authoring
- 更新された `要件定義_プロジェクト名.md` の「要件定義」セクションに、次に実装したい希望内容をユーザーとCodexで追加する。
- 次サイクルの `req_id` / `test_id` を定義し、Step 2へ戻る。

### 8. Record & Handoff
- `target_project_root/result.json` に最低限以下を出力:
  - OS
  - CPU architecture
  - Python version
  - Dependency set/version
  - locale
  - timezone
- 更新内容を以下優先で残す:
  1. AI Spec
  2. Replay inputs
  3. Expected/hash fixtures
  4. Execution environment pinning
- 作業開始前（`phase="before_work"`）と作業完了の最後（`phase="after_work"`）に、「現在の読み上げ対象JSON」へ出力する:
  - `before_work` には「受信したプロンプトをどう解釈したか（作業意図・変更範囲・非範囲）」を記載する。
  - `after_work` には「実際に行った作業と結果要約」を記載する。
  - この要件は `target_project_root` がどのプロジェクトでも必須で適用する（例外なし）。
  - さらに、通常の会話応答を含む「Codexの全応答」で毎回適用する（実装作業の有無を問わない）。
  - 出力先は常に `C:\Users\gonec\codex_workspace\event.json` に固定する。
  - `target_project_root/config/config.json` の `watch_path` は参照しない。
  - 出力先の親ディレクトリが存在しない場合は作成する。
  - 発話安定化のため、`before_work` は単独で先に書き込み、同一ターンで `after_work` を上書きする前に最低1秒待機する。
  - `after_work` 書き込み前に、直前の `event.json` の `phase` が `before_work` であることを確認してから上書きする。
  - `before_work` 出力に失敗した場合、そのターンの作業は開始せず、失敗点と再実行方針のみ報告する。
  - 出力形式は、最低限 `type="readaloud"` と `payload.text` を含む単一JSONとする。
  - `title` / `payload.text` / `keywords` は必ず日本語で記載する（英語を使わない）。
  - `payload.text` には、今回の実装内容を日本語で簡潔に記載する。
  - 読み違い対策として、発話用JSONの `payload.text` は漢字を使わない（ひらがな・カタカナ・英数字・記号のみ）。
  - 上記以外（通常の報告、要件定義書、コードコメント等）は通常どおり漢字を使ってよい。

## Replay / Determinism Policy
- リプレイ入力は Actionレイヤで表現し、キーコード直結を禁止する。
- 実入力と擬似入力は同一の入力集約状態（InputState等）へ収束させる。
- replayファイルは `version` 必須。推奨項目:
  - `metadata`
  - `seed`
  - `fixed_dt`
  - `steps`
  - `expected_ref`
  - `tolerance`
  - `timeout_frames` または `timeout_ms`
- 実行モードは最低限 `headless` / `speed` / `timeout` を提供する。
- 実行後に以下を出力する:
  - 最終状態サマリ（JSON）
  - 重要イベントログ（NDJSON等）
- 互換方針:
  - 原則として旧version読込を維持
  - 破壊的変更は `IMMUTABLE_CHANGE_REQUEST` を必須化

## IMMUTABLE_CHANGE_REQUEST (Required Template)
互換性破壊や恒久変更を入れる場合、以下を必須化する。
- reason（なぜ必要か）
- impact tests（どの `test_id` が影響を受けるか）
- migration deadline（移行期限）
- compatibility note（互換性維持策、または破棄範囲）

## Done Criteria
完了判定は次を全て満たした時点のみ。
1. 影響する全 `req_id` に対応する `test_id` が成功
2. 影響する replay シナリオがある場合は成功し、未整備なら理由付きで `N/A` 記録または新規1本を追加
3. ユーザー手動テスト結果（`OK/NG`）が記録されている
4. `OK` の場合、`要件定義_プロジェクト名.md` に詳細設計追記が完了している
5. `target_project_root/result.json` の必須メタデータが揃っている
6. 変更した契約/期待値に理由と追跡可能性がある
7. 未確定事項と手動確認事項が明示されている
8. 非実装タスク（相談・設計・レビュー等）の場合、未適用項目は理由付きで `N/A` 記録を許可する

## Prohibited Actions
- 要件未更新のまま挙動を変えること
- 失敗テストを理由なく削除・無効化すること
- 範囲外リファクタで差分を広げること
- replayを物理キー依存で固定すること
- ユーザー手動確認前に詳細設計を「確定版」として記録すること

## Expected Agent Output
この節は、実装サイクル完了時の最終報告にのみ適用する。
作業完了時の報告には次を含める。
- 変更機能 / モジュール
- `req_id` - `test_id` 対応表
- 実装差分の要約
- テストとリプレイの実行結果
- ユーザー手動確認の結果（`OK/NG`）と次アクション
- `OK` 時に追記した詳細設計の要点
- 残存リスク / 未確定事項

## AGENTS Change Record
- `change_id`: `CHG-20260214-2347`
- `target_files`: `AGENTS/20_development_rules.md`
- `rules_added_or_updated`: `DEV-RESUME-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・追加ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260214-2347\20_development_rules.md`
- `notes`: `開発再開ターンは認識のみ、実装とテストを禁止`
- `change_id`: `CHG-20260215-1534`
- `target_files`: `AGENTS/20_development_rules.md`, `AGENTS/30_output_rules.md`
- `rules_added_or_updated`: `RULE-DEV-PRIORITY-001`, `RULE-DEV-LITE-001..004`, `RULE-DEV-OPT-001..005`, `RULE-DEV-REASON-001..003`, `RULE-DEV-EVT-001..006`, `RULE-DEV-SEARCH-001..004`, `RULE-OUT-MIN-001..004`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260215-1534\`
- `notes`: `既存ルールを削除せず、条件付き緩和でターン数・出力量・探索量の恒久最適化を追加`
- `change_id`: `CHG-20260216-1639`
- `target_files`: `AGENTS/20_development_rules.md`
- `rules_added_or_updated`: `RULE-DEV-TARGETFILES-001..004`, `RULE-DEV-RESUME-005`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260216-1639\20_development_rules.md`
- `notes`: `開発再開の対象ファイル列挙ルールを固定化し、Resume Recognition Modeに適用条件を追加`
- `change_id`: `CHG-20260216-1646`
- `target_files`: `AGENTS/20_development_rules.md`
- `rules_added_or_updated`: `RULE-DEV-TARGETFILES-001`, `RULE-DEV-TARGETFILES-002`, `RULE-DEV-TARGETFILES-003`, `RULE-DEV-TARGETFILES-004`, `RULE-DEV-RESUME-005`, `RULE-DEV-RESUME-006`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260216-1646\20_development_rules.md`
- `notes`: `開発再開時対象ファイル抽出ルールを明文化固定化`
- `change_id`: `CHG-20260216-1653`
- `target_files`: `AGENTS/20_development_rules.md`
- `rules_added_or_updated`: `RULE-UI-DIALOG-001..006`
- `validation_result`: `PASS`（参照先存在・見出し存在・ルールID重複なし・優先順位矛盾なし）
- `rollback_point`: `C:\Users\gonec\codex_workspace\AGENTS\_snapshots\CHG-20260216-1653\20_development_rules.md`
- `notes`: `全プロジェクト共通の子ダイアログ中心配置を規約化`



