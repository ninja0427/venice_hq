# READ_FILE の範囲・pattern・VPS パス

## 適用条件
ファイルを読む・検索する時。特に大きいファイル、行番号の特定、VPS 上のファイル。

## 手順
- 範囲指定（1始まり・end は含む）:
  `[READ_FILE path="..." start="1100" end="1300"][/READ_FILE]`
- 正規表現で一致行だけ（行番号付き・先頭200件・副作用なし）:
  `[READ_FILE path="..." pattern="def load_eac_baseline|EAC_BASELINE"][/READ_FILE]`
- VPS 上のファイルは絶対パスをそのまま渡す（SSH 経由・ssh コマンド不要）。
  `/opt/` `/home/` `/etc/` `/var/` 始まりが対象。LIST_DIR / CREATE_DIR も同様:
  `[READ_FILE path="/opt/sys_maintainer/src/api.py" pattern="def register"][/READ_FILE]`
- 出力は全行 `%5d| ` の行番号付き。相対パス入力時は結果頭に `相対パスを解決: <絶対パス>`。

## 失敗と回避
- **行番号は表示専用**。REPLACE_IN_FILE の OLD/NEW やコード引用に `123| ` を含めると
  一致しない・壊れる。番号は必ず外す。
- 出力が 50,000 文字を超えると切り詰められる。「後半が読めない」時は同じファイルを
  無指定で読み直さず、`start`/`end` か `pattern=` で絞る。無指定の再読は毎回
  同じ位置で切れてトークンだけ消費する（累積入力警告→自動リセットの主因）。
- 定義位置を探すのに Select-String / grep を RUN_COMMAND で叩かない。`pattern=` で足りる。
- 案内文を自分で書く時、f-string 内に `"` を入れない。`pattern='正規表現'` と `'` で書く。
- CHECKER/SCOUT が渡す `C:\\Developer\\...`（二重区切り）はそのまま通る（内部で畳まれる）。
  自分で二重エスケープを足す必要はない。
- 文字コードは utf-8→utf-8-sig→cp932→latin-1 で自動判定。cp932 以降は注記が付く。
  読めない＝壊れている、とは限らない。
