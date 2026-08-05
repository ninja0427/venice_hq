# VPS へのデプロイ（DEPLOY_FILE）

## 前提

稼働しているのは VPS 上のファイルである。ローカルの案件配下を修正しただけでは
本番には一切反映されない。ローカルはあくまで編集用の作業コピーである。

- apex_update_monitor → `/opt/apex_monitor/`
- sys_maintainer → `/opt/sys_maintainer/`

## 手順

1. ローカルファイルを REPLACE_IN_FILE で修正する
2. VERIFY_SYNTAX でローカルの構文を確認する
3. coder に DEPLOY_FILE を実行させる

```
[DEPLOY_FILE path="C:\Developer\Projects\sys_maintainer\vps_current\src\api.py" remote="/opt/sys_maintainer/src/api.py"][/DEPLOY_FILE]
```

## DEPLOY_FILE が自動で行うこと

- 転送前に VPS 側へ `.bak.<timestamp>` を作成（同一ファイル直近5世代まで保持）
- 転送後に sha256 を照合。不一致ならバックアップから自動復旧
- `.py` は VPS 側の venv python で py_compile を実行。NG なら自動復旧
  （新規ファイルの場合は転送物を削除）

結果は成功・失敗のいずれも機械判定された文字列で返る。自己判断で
「成功した」と報告してはならない。返却された結果文を必ず読むこと。

## 制約

- 転送先は案件ルート配下のみ。`/etc/` 等は拒否される
- リモートパスに `..` やメタ文字（`;` `&` `|` `>` `` ` `` `$` 等）を含めると拒否される
- `.bak.` を含むファイル名は転送先にできない
- scp / ssh を RUN_COMMAND で実行しても Tier2 で拒否される
- **サービスの再起動は行わない**。systemctl 等は実行できない。
  転送は反映の準備までであり、再起動は人間が実施する

## VPS 上のファイルは読めない

READ_FILE / LIST_DIR / CREATE_DIR に `/opt/` 等の POSIX 絶対パスを渡すと拒否される。
読取系も例外ではない。Windows では `os.path.isabs("/opt/x")` が True になり
`C:\opt\x` へ化けるため、`_resolve_path` が全経路で明示拒否する。

```
[LIST_DIR path="/opt/apex_monitor"][/LIST_DIR]
→ パス拒否: VPS 側パスをローカルとして解決できません
```

VPS の状態は確認できない。確認が要る場合は Tier1 として pending.md に起票し、
待たずに先へ進む。VPS への反映は DEPLOY_FILE のみ。

## ファイル形式の注意（転送前に必ず）

Windows で編集したファイルは改行が CRLF のままになる。systemd の
`EnvironmentFile` は CRLF を読めず起動に失敗する。転送する前に LF へ直すこと。

`.env` を編集する場合の規約：

- 値をクォートで囲まない（systemd はクォートを値の一部として保持する）
- 改行は LF のみ、パーミッションは 600
- 置き場所は `/opt/<project>/env/*.env`

`.py` `.sh` `.env` はいずれも LF で保存する。
