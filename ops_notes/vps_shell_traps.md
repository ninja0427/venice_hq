# VPS シェルの落とし穴（運用者専用・周回に注入しない）

分隊は ssh / RUN_COMMAND で VPS シェルを実行できない（Tier2 拒否）。
本ファイルは人間および CLI セッション側の手作業用。

## SSH / クォート

OpenSSH はリモートへ渡す際に二重引用符を剥がす。PowerShell は `"$(...)"` を
ローカルで展開してしまう。

対処：リモートコマンドは単引用符で囲む。複数行は次の形にする。

```
ssh host 'bash -s' << 'EOF'
...
EOF
```

## 不可視文字

`/proc/PID/environ` は NUL 区切りのため、素の grep では拾えない。

対処：`strings` または `grep -a`。ファイル内容の確認は `cat -A` / `xxd`。

## 参照

- 転送手順とファイル形式規約 → `HQ\skills\vps_deploy.md`
