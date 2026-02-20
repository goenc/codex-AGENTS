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

## Commit Message Policy
- `RULE-DEV-COMMITMSG-001` すべてのコミットは、コミットメッセージを日本語で記述し、必ず次の2層構造にする。
  - 1行目: 要約（何をやったか）
  - 2行目以降: 変更内容（最低1行以上）
- `RULE-DEV-COMMITMSG-002` 2行目以降の各行は、必ず `- 何を: ` で開始する。これ以外のラベル（例: 影響/理由/背景/検証/Why/How/Notes）は禁止する。
- `RULE-DEV-COMMITMSG-003` 1機能のみの実装で詳細が無い場合、2行目以降の内容は1行目と同一内容でもよい。ただし2行目以降は省略禁止（最低1行必須）。
- `RULE-DEV-COMMITMSG-004` `1req=1commit` の粒度方針と整合するよう、2行目以降は当該reqの変更点のみを列挙し、無関係な事項を混ぜない。
- `RULE-DEV-COMMITMSG-005` 1行目（要約）の直後には、必ず1行の空行を挿入しなければならない。空行なしで2行目から `- 何を: ` を開始してはならない。なお本要件適用時、`RULE-DEV-COMMITMSG-002` の適用開始行は3行目以降とする。
- `RULE-DEV-COMMITMSG-006` コミットメッセージ内の改行コードはLFのみを使用しなければならない。CRLFを含むメッセージ生成を禁止する。

## End-of-Line Policy
- `RULE-DEV-EOL-001` リポジトリ内部の改行コードはLFに統一する。
- `RULE-DEV-EOL-002` 改行コード正規化が必要な場合は `git add --renormalize .` を使用して正規化コミットを行う。

## Source-Driven Determination
- `RULE-DEV-SOURCE-001` 仕様確定はソースコードを第一優先とする（Rustでは `Cargo.toml` + `src/`）。
- `RULE-DEV-SOURCE-002` `不変条件_<project>.md` に反する変更は実装しない。
- `RULE-DEV-SOURCE-003` `再開_<project>.md` は探索制御にのみ使用し、仕様そのものの根拠には使わない。

## Lightweight Verification
- `RULE-DEV-VERIFY-001` 既定検証は軽量検証とする。
- `RULE-DEV-VERIFY-002` Rustの軽量検証は `cargo fmt --all` と `cargo test`（または影響範囲テスト）を基本とする。
- `RULE-DEV-VERIFY-003` 非Rustは `entry_points` に対応する最小テスト/検証コマンドを実行する。
- `RULE-DEV-VERIFY-004` 検証失敗時はコミットしない。

## Build Procedure (Windows EXE Lock Prevention)
- `RULE-DEV-BUILDWIN-001` Windowsでビルドする場合、対象アプリ（exe）の起動有無に関係なく、ビルド開始前に毎回プロセス終了を試行する。
- `RULE-DEV-BUILDWIN-002` 同名プロセス巻き込み防止のため、可能な限り `ExecutablePath` のフルパス一致で終了対象を特定する。
- `RULE-DEV-BUILDWIN-003` 対象プロセスが存在しない場合はエラーとして扱わず、無害として続行する。
- `RULE-DEV-BUILDWIN-004` 本運用は管理者権限で起動しない前提とし、同一ユーザー権限で停止可能な範囲を対象にする。
- `RULE-DEV-BUILDWIN-005` Codexのビルド前フックとして `pwsh -File build.ps1` の実行を推奨する。
- `RULE-DEV-BUILDWIN-006` PowerShell実装例は次を基準とする。

```powershell
# build.ps1（例）
$exePath = "C:\path\to\YourApp.exe"

# 実行中なら終了（存在しなくても無視して続行）
Get-CimInstance Win32_Process |
  Where-Object { $_.ExecutablePath -eq $exePath } |
  ForEach-Object { Stop-Process -Id $_.ProcessId -Force -ErrorAction SilentlyContinue }

# ここから通常ビルド
cargo build
```

```powershell
# Codexビルド前フック実行例
pwsh -File build.ps1
```

## Contract Promotion
事故や再発防止が必要な場合のみ、不変条件への昇格を検討する。

- `RULE-DEV-PROMOTE-001` 昇格検討は、障害再発防止または破壊的回帰の予防が必要な場合に限定する。
- `RULE-DEV-PROMOTE-002` 昇格候補は次を全て満たすこと。
  - 失敗条件を観測可能に判定できる。
  - 実装詳細に依存しない。
  - 将来の改修でも維持すべき契約である。
  - `must` / `must not` の契約文で短く記述できる。
- `RULE-DEV-PROMOTE-003` `RULE-DEV-PROMOTE-002` を満たさない事項は不変条件へ昇格しない。
