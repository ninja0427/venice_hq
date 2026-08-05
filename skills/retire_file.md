# ファイルの除去は RETIRE_FILE（物理削除の手段は無い）

## 適用条件
不要・廃止ファイルを消したい時。del / rm / Remove-Item は使えない。
許可役は CODER のみ（SCRIBE / LIBRARIAN には無い）。

## 手順
1. 退避タグを発行する。対象は `_retired\<timestamp>\` へ**移動**される（可逆・Tier0）:
   `[RETIRE_FILE path="C:\Developer\Projects\<案件>\vps\old_module.py"][/RETIRE_FILE]`
2. 退避後は import 文・呼び出し箇所の参照が壊れる。`[REPLACE_IN_FILE]` で直す。

## 失敗と回避
- 保護6種は退避不可（basename 一致で拒否）:
  status.md / pending.md / HANDOFF.md / README_CORE.md / README_REF.md / ledger.md。
- `_snapshot` / `_retired` 配下も退避対象にできない。
- 退避先は `_snapshot` ではない。スナップショットは20世代で古い分が削られるため、
  そこへ退避すると20周回後に消える。
- 「削除せよ」という orders が来ても物理削除ツールは無い。RETIRE_FILE で退避して
  完了扱いにする。RUN_COMMAND での削除は Tier2 で通らない。
- 退避しただけで参照修正を忘れると次周回で ImportError になる。退避と参照修正はセット。
