# 認証情報の Git 追跡（2026-08-05）

運用者向け。分隊は読まない。恒久ルールは `CLAUDE.md` § Secrets & Git に転記済み。

## 何が起きたか

`apex_update_monitor` リポジトリに、実値入りの env 3ファイルが追跡されていた。

| ファイル | 実値 |
|---|---|
| `vps/systemd/bot.env` | `DISCORD_BOT_TOKEN`（72文字）/ `INGEST_TOKEN`（47文字） |
| `vps/systemd/pics.env` | `INGEST_TOKEN` |
| `local/.env` | `INGEST_TOKEN` |

初回コミット `ff51849`（2026-07-16）から 2026-08-05 の push まで履歴に残っていた。
リポジトリは private だが、GitHub 上に実値が存在した状態だった。

同時に `.venv/`（依存一式）と `.docs_backup/`（.bak 118件）も追跡されており、
追跡解除時の差分は 4,018ファイル・827,352行に達した。

## 原因

**1. `.gitignore` は未追跡ファイルにしか効かない。**
`.gitignore` に `*.env` は書かれていた。しかし `bot.env` / `pics.env` / `local/.env` は
それより前にコミット済みで、追跡済みファイルは `.gitignore` の対象外になる。
「ignore に書いた＝安全」という思い込みが20日間チェックを止めた。

**2. リポジトリ作成時に `.gitignore` が無かった。**
`.venv/` `.docs_backup/` `*.env` を登録する前に `git add` が走り、全部入った。

**3. 秘密の受け渡し経路が定義されていなかった。**
新トークンが2回、IDE の選択範囲経由で AI の会話コンテキストに入った。
`sed` のコマンドラインに直接書けば VPS の `~/.bash_history` にも残る。

**4. PowerShell に `&&` を含むコマンドを渡した。**
Windows PowerShell 5.1 は `&&` を解釈しない。`cd` が失敗し、後続コマンドが
別リポジトリ（`apex_update_monitor`）で実行された。
このとき出力された `git ls-files` から漏洩が判明したため、事故が発見の契機になった。

## 発見の経緯

HQ を git 管理下に置く作業で、`cd C:\Developer\Projects\HQ && ...` を PowerShell へ渡した。
`&&` でパースエラーになり `cd` が実行されず、後続の `git add` / `commit` / `ls-files` が
`apex_update_monitor` で走った。その `git ls-files` の出力に `.venv/` と env が並んでいた。

## 対処（実施済み）

1. `DISCORD_BOT_TOKEN` を Discord Developer Portal で Reset（対象 Bot: `EAC_Analyzer#6858`）
2. `INGEST_TOKEN` を VPS 上で `openssl rand -hex 32` により再生成し、3箇所へ同時反映
3. `systemctl restart apex-bot` で新トークンでの再接続を確認
4. `.gitignore` に `*.env` / `!*.env.example` / `.venv/` / `.docs_backup/` を追加
5. `git rm -r --cached` で追跡解除（作業ツリーのファイルは残す）→ `75ac507`

## 未対処

- **履歴に旧実値が残る。** `git filter-repo` での除去が必要。
  トークンはローテーション済みのため緊急度は下がったが、解消していない。
- 新 Discord トークンが会話コンテキストに入ったため、**再 Reset が必要**。

## 再発防止

- 新規リポジトリは `git init` の直後に `.gitignore` を置く。最低限 `.venv/` `*.env`。
- 既存リポジトリでは `git ls-files` を定期的に見る。`.gitignore` を信用しない。
- 秘密は VPS 上で直接編集する。値を AI へ渡さない。確認は `grep -c` の件数のみ。
- `sed` で秘密を入れる場合は `set +o history` で囲む。
- PowerShell へ渡すコマンドに `&&` を使わない。1行ずつ実行する。
  -> `HQ\skills\powershell_basics.md` / `HQ\ops_notes\vps_shell_traps.md`

## 参照

-> CLAUDE.md § Secrets & Git（恒久ルール2行）
-> apex_update_monitor\HANDOFF.md 指摘事項ログ
