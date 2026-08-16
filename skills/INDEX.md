# 共通スキル目次

この目次は全周回で固定部として読み込まれる。本文は必要時に READ_FILE で個別に読む。
本文の場所：`C:\Developer\Projects\HQ\skills\<ファイル名>`

## カテゴリ固定リスト

分隊は status.md フロントマターの `categories:` に、下記 `attribute:` 系の名前のみを
リスト形式で記載する。新しい名前を勝手に作らない。追加が要る場合は Tier1 として
pending.md に上げる。

記載例：

```yaml
categories: [powershell, git, deploy]
```

- attribute:powershell … PowerShell 作法・コマンド実行
- attribute:git … Git 操作・履歴・ブランチ
- attribute:report … 索引再構成・レポート整備
- attribute:deploy … VPS・デプロイ・常駐サービス
- attribute:api … 外部 API 呼出・認証・レート制限
- attribute:design … ポスター・バナー・図版の制作。DESIGNER を持つ分隊のみ
- attribute:hermes … Hermes 同梱スキルの参照。categories への申告は不要で、
  どの案件からでも「詰まった時」に引いてよい唯一の例外行

`always` はカテゴリ名ではなく本体側の固定挙動。案件の申告に依存せず無条件でロードする。
フロントマターに `always` と書かない。

## 運用規約

- エントリ表に載っていない本文は読まない。
- カテゴリ欄が `always` の本文は、案件を問わず周回頭で本文まで無条件にロード済み。改めて読む必要はない。
- `attribute:` 系は先読みしない。自案件の `categories:` と一致する行の存在だけ把握し、道中で詰まった時に READ_FILE で本文を読む。
- 1つの本文は複数カテゴリに属してよい。カテゴリ欄はリストで書く。
- このファイルへの書込は人間の手作業。分隊は書き換えない。

## エントリ表

| ファイル名 | カテゴリ | 要約 |
|---|---|---|
| powershell_basics.md | always | PowerShell 作法・パス・ファイル生成・検証の基本規約 |
| vps_deploy.md | attribute:deploy | DEPLOY_FILE によるVPS反映手順・バックアップと自動復旧の挙動 |
| run_command_alternatives.md | always | RUN_COMMAND は既定 Tier2。読取検索構文検証削除反映の意図別代替タグ表 |
| replace_in_file.md | always | 既存ファイル編集は REPLACE_IN_FILE。WRITE_FILE は新規のみ8192 切断の真因 |
| read_file_usage.md | always | start/endpatternVPS 絶対パス行番号は表示専用切り詰め時の対処 |
| retire_file.md | always | ファイル除去は RETIRE_FILE で _retired へ退避。保護6種CODER のみ参照修正 |
| orders_consumption.md | always | orders は全文転記済み案件配下を探索しない到達不能は報告して打ち切り |
| subagent_delegation.md | always | 役別ツール許可読取専用役の締め方finish_reason=length 時の継続 |
| status_and_ledger.md | attribute:report | status.md 9フィールドと state 値台帳へ書けるのは CLI のみ |
| poster_design.md | attribute:design | 制作案件の進め方。GEN_IMAGE の使い方日本語は画像に描かせない画像モデルの使い分け上限1280px |
| lessons.md | always | 分隊が踏んだ失敗の集約。周回頭に必読。同じ失敗を繰り返さない。書込は status.md の ## 教訓 経由で CLI が転記 |
| hermes_catalog.md | attribute:hermes | Hermes 同梱79スキルの目次。分野で引き絶対パスの SKILL.md を READ_FILE。79件中56件はそのまま使える |
