# サブエージェント委譲と切断継続（LEAD）

## 適用条件
LEAD が役別サブエージェント（SCOUT/CODER/CHECKER/SCRIBE/LIBRARIAN）へ委譲する時、
および自分の応答が finish_reason=length で切れた時。

## 手順
- 役別の許可ツール（許可外タグは実行前に除去され denied になる）:
  - SCOUT / CHECKER / ADVISOR / REVIEWER … READ_FILE / LIST_DIR / VERIFY_SYNTAX（読取専用）
  - SCRIBE / LIBRARIAN … 上記＋ WRITE_FILE / REPLACE_IN_FILE / CREATE_DIR / RUN_COMMAND
  - CODER … 上記＋ RETIRE_FILE / DEPLOY_FILE
- 切断（finish_reason=length）時は、長い説明やコード全文を書き出さず、**次の1アクション
  だけをタグで**出す。本文は CODER へ委譲し LEAD 自身は書かない。

## 失敗と回避
- 読取専用役（SCOUT 等）に「WRITE_FILE で完了させろ」と促さない。同一ファイルへ
  繰り返し書こうとして空転する（実測: 1周27万トークン）。読取役の締めは
  「ツールを使わず報告本文だけを出力して終了」。
- CODER が length / tool_calls で失敗した時、LEAD が全文 WRITE_FILE で肩代わりしない
  （断片上書き・切断の連鎖になる）。作業を分割し、REPLACE_IN_FILE で1箇所ずつ渡す。
- 切断応答をモデルが再構成する際、同一タグを数十〜数百回反復する（results count=220 等）。
  読取系は自動で重複除去されるので気にしない。書込系は除去されないので同じ書込を
  何度も並べない。
- 自分の応答が3回連続で length になると自動停止する。1発言に詰め込みすぎない。
- サブエージェントが「作成しました」と報告するがツールを出していない時は完了を信じない
  （TOOL_SYNTAX 不注入時の虚偽報告）。ツール実行の痕跡で判定する。
