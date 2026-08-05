# 既存ファイルの編集は REPLACE_IN_FILE（WRITE_FILE は新規のみ）

## 適用条件
既に存在するファイルの一部を変更する時。全文を WRITE_FILE で書き直さない。
WRITE_FILE は新規作成専用。許可役は CODER / SCRIBE / LIBRARIAN。

## 手順
1. `[READ_FILE path="..."]` で対象箇所を読み、OLD に**空白・インデント・改行を1文字も
   変えず**転記する（行番号は含めない）。
2. 次の形で発行する（区切りは `===`）:
   [REPLACE_IN_FILE path="C:\Developer\Projects\<案件>\...\file.py"]
   <<<OLD
   置換前の既存コード（一意に1件だけ一致する範囲）
   ===
   置換後のコード
   >>>NEW
   [/REPLACE_IN_FILE]
3. 変更後は `[VERIFY_SYNTAX path="..."]` で構文確認。
- OLD が一意に1件一致した時だけ実行。CRLF / BOM / エンコーディングは既存ファイルの
  ものが自動で保たれる。結果に変更前後のバイト数が返る。
- 複数箇所を変える時は REPLACE_IN_FILE を複数回に分ける（1回1箇所を一意に）。

## 失敗と回避
- OLD が0件一致 → `denied`。READ_FILE で現物を再確認し、コピー時に落ちた空白・全角化けを
  疑う。行番号を混入させない。
- OLD が2件以上一致 → `denied`。前後の行を足して一意にする。
- 直前の REPLACE で行がずれ、後続の OLD が一致しなくなる。**1箇所直すたびに
  `pattern="def 関数名"` で現在位置を読み直してから次の OLD を作る**（実際に行番号ずれで
  変更3が拒否された）。
- 全文 WRITE_FILE を選ぶと出力が 8192 トークンで切れて途中で終わる。8192 は
  `venice_client.chat` の既定 max_tokens でありモデルの停止不良ではない。分割ではなく
  REPLACE_IN_FILE で部分置換すれば張り付かない。
- 2000字以上の既存ファイルを5割未満に縮める全文 WRITE_FILE は縮小ゲートで拒否される
  （切断された断片での上書き破壊を防ぐため）。
- `apply_*.py` / `patch_*.py` のような適用スクリプトを書いて RUN_COMMAND で回そうとしない
  （Tier2 で弾かれ周回が空転する）。
