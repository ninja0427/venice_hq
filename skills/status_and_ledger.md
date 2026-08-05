# status.md の書き方と台帳（ledger）への非書込

## 適用条件
周回の状態を `_report\status.md` に記録する時、または決定を残したい時。
SCRIBE / LEAD 向け。

## 手順
- status.md はフロントマター現況上書き式。フィールドは9個固定:
  `squad / project / categories / state / last_order / blocker /
  pending_approval / budget_used / updated`。
- `state` の値は `idle` / `running` / `waiting_hq` / `blocked` / `stopped` のいずれか。
- 記録したい決定は status.md の**直近完了欄**に書く。周回完了時に CLI が台帳へ自動転記する。
- 編集は `[REPLACE_IN_FILE]`（フロントマターの1フィールドだけを一意置換）。

## 失敗と回避
- **台帳 `HQ\ledger.md` には誰も書けない**。書けるのは CLI のみ。案件配下に ledger.md を
  作るのも HQ の ledger.md を書換えるのも execute_write_file 冒頭で拒否される。
  RUN_COMMAND / Add-Content 経由の追記も同じく不可。台帳へ追記しようとして3回拒否→Tier2
  停止、が過去の典型的な空転パターン。
- `categories` は AI が消しても `_preserve_status_fields()` が既存値へ差し戻すので、
  上書きで欠落しても事故ではない。逆に勝手に増やしても保持されない。
- `squad` は記録用の値。**割当の判定・表示に使わない**。UNKNOWN が入っていても
  それを正として扱わない。権威は `models.json:assignments`。
- rc=2 で終わった周回は status が `running` のまま残ることがある（write_status を通らない）。
  UI 表示が実態とずれても状態そのものの異常ではない。
