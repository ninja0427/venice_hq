# RUN_COMMAND を使わない・意図別の代替タグ

## 適用条件
シェルコマンド（PowerShell / cmd）を実行したくなった時。読取・検索・構文検証・
削除・VPS 反映は専用タグがある。RUN_COMMAND は既定 Tier2 で通らない。

## 手順
やりたいこと → 使うタグ（RUN_COMMAND ではない）:
- ファイルを読む / 行番号を知る → `[READ_FILE path="..." start="N" end="M"][/READ_FILE]`
- 関数・定数の定義位置を検索（grep / Select-String 相当） →
  `[READ_FILE path="..." pattern="正規表現"][/READ_FILE]`（一致行のみ・先頭200件・Tier判定なし）
- .py / .json の構文検証（py_compile 相当） → `[VERIFY_SYNTAX path="..."][/VERIFY_SYNTAX]`
- 既存ファイルの編集 → `[REPLACE_IN_FILE]`（別スキル）
- ファイルの除去（rm / del / Remove-Item 相当） → `[RETIRE_FILE path="..."][/RETIRE_FILE]`
- VPS への反映（scp / ssh 相当） → `[DEPLOY_FILE path="..." remote="..."][/DEPLOY_FILE]`
- ディレクトリ一覧（dir / ls 相当） → `[LIST_DIR path="..."][/LIST_DIR]`

どうしても RUN_COMMAND が要る場合、Tier0 で通るのは読取専用18件の前方一致のみ：
git status/log/diff/branch/show/remote -v, ls, dir, cat, type, findstr, pwd, where,
echo, python -V, python --version, pip list, pip show。
`> < | & ; ` `` ` `` $( %(` の8文字を1つでも含むと即 Tier2。

## 失敗と回避
- RUN_COMMAND は同一周回で3回拒否されると以後即遮断される（連続2周回で停止）。拒否の変奏
  （`|`→`;`→`Test-Path`で囲む→`python -c`→スクリプトファイル書き出し）を繰り返さない。
- 拒否結果にコマンド文も理由も載らないのは仕様（失敗形を context に残さないため）。
  同じコマンドを言い換えて再送しても通らない。上の代替タグへ切り替える。
- READ_FILE が切り詰め（50,000字超）で「定義位置が分からない」時に Select-String へ
  逃げない。`pattern=` 属性で該当行だけ絞れる。
- 構文検証目的の `py_compile` / `ast.parse` / `json.tool` は自動で VERIFY_SYNTAX に
  振り替わる。`&` や `|` を付けても振替が先に効く。最初から VERIFY_SYNTAX を出す。

