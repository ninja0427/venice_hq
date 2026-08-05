# 関数全文置換の照合（powershell_basics.md の補完）

## 適用条件
venice_cli 本体のコードを関数単位で全文置換した後の検証。
（構文検証・dir 確認の基本は powershell_basics.md。ここは置換特有の事故のみ。）

## 手順
1. 置換後、変更した関数の `return` 文数を数えて照合する:
   `python -c "import inspect,orchestrator as o; print(inspect.getsource(o.Orchestrator._resolve_path).count('return'))"`
   期待値: `_resolve_path`=4 / `_detect_shrink_overwrite`=5 / `execute_read_file`=10 /
   `execute_replace_in_file`=8 / `execute_retire_file`=7 / `execute_verify_syntax`=8 /
   `_redirect_syntax_command`=4 / `execute_deploy_file`=14。
2. 複数箇所を同時に変えたら出現回数を数える:
   `python -c "t=open('agents.py',encoding='utf-8').read(); print(t.count('REPLACE_IN_FILE'))"`
   照合値の例（S39）: TOOL_SYNTAX=5 / CODER_SYSTEM_PROMPT=5 / LEAD_SYSTEM_PROMPT=7。

## 失敗と回避
- 全文置換で範囲が下にずれると早期 return と末尾 return を巻き込んで消し、`ast.parse` は
  通過したまま実行時に `TypeError: cannot unpack non-iterable NoneType`。return 数の照合で
  検出する。`build_auto_prompt` のように return が2つある関数は特に注意。
- 照合の目印は**新規に追加した記号列**を選ぶ。既存語を数えると「変わっていない」ことに
  気付けない（`rec.pop` の出現数0で未反映を即検出できた）。
- `### READ_FILE` 等の同一文面は LEAD / CODER / TOOL_SYNTAX の3箇所に存在する。1箇所だけ
  直すと不整合。3箇所同時に置換し、置換数を数えて確認する（過去に1箇所未置換・1箇所
  インデント崩れのまま ast.parse を通過した）。
- IndexOf の目印に日本語を使うとコピー時に「・」等が落ちて -1 を返す。ASCII 語で目印を打つ。
