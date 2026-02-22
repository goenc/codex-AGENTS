# End Phase Skill

## 目的
- End Phase の実行順序を管理する
- 本スキルは git commit を実行しない

## 起動条件
- Implementation Phase 完了後に実行する

## 実行順序

1. `skills/end_phase_commit.md` を参照してコミット関連ファイルを生成する
2. `skills/speech_output.md` を参照して発話用ファイルを生成する

## 適用範囲

- End Phase では上記二つのスキルを順に適用する
- 発話のみ必要な場合は `skills/speech_output.md` を単独で利用できる
- 発話用ファイルはルート runtime へ出力し、コミット関連ファイルは対象プロジェクト runtime へ出力する

## 失敗時

- 下位スキルで失敗した場合は処理を中断し失敗理由を対話で明示する

## 禁止事項

- git commit の実行
- 下位スキルに定義済みの規則を上書きすること
