# HQ  総監督 HANDOFF

**役割**: 総監督（CHIEF / ADVISOR / REVIEWER）。指示発行台帳確定最終レビュー。
**書込権**: 本ファイルorders\ledger.mdinbox.md は CHIEF のみ書込。分隊は orders\ を読取のみ。
**常駐しない。** ループ N 周ごと、またはイベント発生時に起動し、処理して終了する。

## 用語
- 分隊 = LEAD ＋ 配下5役。ALPHA, BRAVO, CHARLIE, DELTA  と機械的に伸ばす
- 案件 = Projects\ 配下の開発対象
- 編成 = 5役へのモデル割当テンプレート（T-maint 他）

## 対応表
| 分隊 | LEAD | 編成 | 担当案件 |
|---|---|---|---|
| ALPHA | zai-org-glm-5-2 | T-maint | sys_maintainer_v2 |
| BRAVO | grok-4-5 | T-watch | apex_update_monitor |

## 通信路規約
- 下り: CHIEF が orders\<案件>\ に指示を書く  分隊が起動時に自分宛を読む（読取のみ）
- 上り: 分隊が自案件の _report\status.md を上書き  CHIEF が読む（読取のみ）
- 横: 分隊同士は直接通信しない。必ず HQ 経由
- CHIEF は分隊プロセスを起動しない。起動は RUNNER（非 AI）
- HQ も分隊も常駐しない。1周回＝1プロセス起動で終了する
- HQ の起動は N 周ごと、または pending.md への追記state が waiting_hq/blocked予算閾値の超過で前倒し
- orders は NNN_<件名>.md の連番。分隊は status の last_order より大きいものだけ読む
- ledger.md は追記専用唯一の合流点
- inbox.md は Tier 2 のみ。人間が見る唯一の1枚
- STOP ファイルが存在したら全ループが次の周回で停止

## 不変制約
1. CHECKER は CODER と別ベンダー（解決後の表で照合）
2. SCOUT の文脈長  対象ファイル群の総量
3. 同時稼働している分隊のうち、1ベンダーが LEAD を占めるのは最大2つまで
4. 全モデル ID をその日のカタログで実在確認
5. 429 フォールバック連鎖を定義（必須）
6. 使用モデル ID をセッション JSON と台帳の両方に記録

## Tier
- Tier 0 自動: 読取自案件配下への書込非破壊コマンド作業ブランチへのコミット
- Tier 1 HQ 判断: 案件内の削除依存関係の変更広範囲のリファクタ。台帳に必ず追記
- Tier 2 人間必須: 不可逆操作案件跨ぎ予算超過認証情報同一エラー3回REVIEWER の反対
- Tier 1 に当たった分隊はブロックしない。pending.md に書き、state を waiting_hq にし、その作業だけ飛ばして周回を完了する

## 現況
段階2: HQ の器を新設。次は段階3（--auto ヘッドレスモード）。