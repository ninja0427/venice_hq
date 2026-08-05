# 文字コードと改行（BOM / CRLF / CP932）

## 適用条件
ファイルの構文検証・文字列比較・書込が理由不明で失敗する時。Windows では
CP932・UTF-8 BOM・CRLF の3混在が常態。読む側で吸収する。

## 手順
- 書込は常に BOM 無し UTF-8 で統一する:
  `[System.IO.File]::WriteAllText($path, $text, (New-Object System.Text.UTF8Encoding $false))`
- BOM 付きが前提のファイル（既存の vps_bot.py 等）を REPLACE_IN_FILE で編集する時は、
  BOM は自動で保持されるので気にしない。
- BOM 付きを Python で読む時は `encoding='utf-8-sig'`。破損復元後の ast.parse も同様。

## 失敗と回避
- UTF-8 BOM の `﻿` が混入すると JSON パース・文字列比較が壊れ、`ast.parse` は
  `invalid non-printable character U+FEFF` で落ちる。VERIFY_SYNTAX は utf-8-sig で読むため
  BOM 付きでも誤 NG にならない（自前の py_compile 迂回は不要）。
- CRLF ファイルは行末に `\r` が残り正規表現・比較が不一致になる。比較前に
  `splitlines()` / `.rstrip('\r')` で正規化する。PowerShell の `-replace '(?m)^...$'` は
  CRLF に一致しない。
- **PowerShell のパイプは BOM を付与する**。`curl.exe ... | python -c "json.load(...)"` は
  `Unexpected UTF-8 BOM` で失敗する。HTTP 取得は `urllib.request` で直接行う。
- パイプを通した日本語・`Get-Content` の日本語表示化けは既知の非問題。文字化けを理由に
  再確認しない。照合は ASCII（関数名・定数名）で行う。
- CP932 ファイルを他所で読むと `UnicodeDecodeError`。READ_FILE / VERIFY_SYNTAX は
  自動判定するが、自前スクリプトでは encoding を明示する。
