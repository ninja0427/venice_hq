# PowerShell 作法（always）

対象：Windows PowerShell 上でのコマンド実行・ファイル生成・検証。案件を問わず適用する。

## コマンド実行

- 作業は必ず `cd C:\Developer\Scripts\venice_cli` から始める。
- 複数行の `python -c "..."` は PowerShell で構文エラーになる。1行に収めるか、スクリプトファイルを作って実行する。
- `Select-String` / `Get-ChildItem` は PowerShell 専用。cmd では `findstr` / `dir`。
- `.bat` の中身を PowerShell に直接貼らない。ファイルへ書き込んでから実行する。
- バッチの括弧ブロック内では `shift` 後の `%~1` が更新されない。`goto :label` 方式にする。

## パス

- `[System.IO.File]::ReadAllText()` / `WriteAllText()` は `cd` に追従しない。絶対パスを渡す。
- `.Substring(<固定長>)` でパスを切らない。短いパスで落ちる。相対化は `Resolve-Path -Relative` を使う。

## ファイル生成

- here-string `@'...'@` ＋ `[System.IO.File]::WriteAllText('<絶対パス>', $b, (New-Object System.Text.UTF8Encoding $false))`。
- here-string は終端 `'@` が行頭列にないと本文が空になる。生成後は必ず `(Get-Item '<パス>').Length` でバイト数を確認する。

## 検証

- 検証は外部 PowerShell で行う。CLI 内の `RUN_COMMAND` に自己検証させると誤判定する。
- `ast.parse` はインデント階層・定数名のタイプミス・`%` の個数不一致を検出しない。置換後は `import <mod>; dir(<mod>)` で定数名の実在も確認する。
- `if __name__` ガードは常にファイル最終行。新規定数の追記先も必ずファイル最終行。
- `Get-Content` の日本語表示化けは既知事象。文字化けを理由に再確認しない。
- `git commit` の後は `git log --oneline -1` と `git status --short` で着地を確認する。
