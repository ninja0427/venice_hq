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

## sys_maintainer 側（同日・より深刻）

`apex` の調査中に、`sys_maintainer` の `.env` と `server/.env` も追跡されていたことが判明した。
`git ls-files` には出なかった。既にステージ削除済みで index から外れていたため。
**追跡解除済みでも履歴には残る。`git log --all -- <path>` で確認すること。**

| キー | 用途 | ローテート時の影響 |
|---|---|---|
| `PAYLOAD_MASTER_KEY` | ペイロード AES-GCM の master | 既存暗号化物が復号不能。実データ 0 件のため無害 |
| `AGENT_TOKEN_SIGNING_KEY` | JWT HS256 署名 | access token（1h）のみ失効。refresh は CSPRNG で鍵非依存＝再登録不要 |
| `PAYLOAD_URL_SIGNING_KEY` | 署名付き URL | 有効期限 5分で自然回復 |
| `HWID_PEPPER_KEY` | HWID の HMAC pepper | **既存 HWID ハッシュが照合不能**。移行手順が要る |
| `BOT_SERVICE_AUTH_KEY` | Bot↔API 内部認証 | 両ユニット同時 restart で即時 |
| `DISCORD_BOT_TOKEN` | sys 側 Bot | 影響なし |

`HWID_PEPPER_KEY` には自動移行経路がある。`routes/register.py` L85-99 が
HWID 不一致時に `discord_uid` で agent を引き、閾値未満なら新 pepper のハッシュへ更新する。
つまり **agent 1台につき mismatch カウンタを1消費して自動移行する**。
今回は登録済みがテスト用2件のみだったため `revoked_at` を立てて無効化し、
実機は新規登録として入る形にした（カウンタ消費なし）。

影響の小さい順に実行するのが原則。
`DISCORD_BOT_TOKEN` / `PAYLOAD_URL_SIGNING_KEY` / `BOT_SERVICE_AUTH_KEY` →
`AGENT_TOKEN_SIGNING_KEY` → `PAYLOAD_MASTER_KEY` → `HWID_PEPPER_KEY`。

## 未対処

- 新 Discord トークンが会話コンテキストに入ったため、**再 Reset が必要**。
  Discord Developer Portal 上の手作業。ここだけが残っている。

## 再点検（2026-08-16 実測）

履歴の除去は完了していた。上の「未対処」から履歴の項を落とした。

| 確認項目 | apex_update_monitor | sys_maintainer | HQ | venice_cli |
|---|---|---|---|---|
| `*.env` の追跡 | なし | なし | なし | なし |
| 履歴に残る `*.env` | なし | なし | なし | なし |
| `.gitignore` の `*.env` | あり | あり | 白リスト方式で到達不可 | あり |
| origin と一致 | 一致 | 一致 | — | — |

- `ff51849`（初回コミット）と `75ac507`（追跡解除）はローカルにもリモートにも存在しない。
  履歴書き換え後に force-push 済みで、`main` は `b4bedee` で origin と一致している。
- 追跡されている env 系は雛形のみ。`local/.env.example` は `INGEST_TOKEN=your_ingest_token_here`、
  `alpha.env.example` は `ALPHA_DISCORD_TOKEN=`（空）。実値なし。
- `check_tokens.sh` は sqlite3 の SELECT 文のみ。値の埋め込みなし。
- 点検コマンド:
  `git log --all --pretty=format: --name-only --diff-filter=A | sort -u | grep -E '\.env$'`
  ※ `git ls-files` だけでは足りない。理由は上の「再発防止」を見ること。

## 再発防止

- 新規リポジトリは `git init` の直後に `.gitignore` を置く。最低限 `.venv/` `*.env`。
- 既存リポジトリでは `git ls-files` を定期的に見る。`.gitignore` を信用しない。
- `git ls-files` は index の現在値しか見ない。**ステージ削除済みの秘密は出てこない。**
  過去分は `git log --all --diff-filter=A -- '*.env'` で追う。
- 秘密は VPS 上で直接編集する。値を AI へ渡さない。確認は `grep -c` の件数のみ。
- `sed` で秘密を入れる場合は `set +o history` で囲む。
- PowerShell へ渡すコマンドに `&&` を使わない。1行ずつ実行する。
  -> `HQ\skills\powershell_basics.md` / `HQ\ops_notes\vps_shell_traps.md`

## 参照

-> CLAUDE.md § Secrets & Git（恒久ルール2行）
-> apex_update_monitor\HANDOFF.md 指摘事項ログ
