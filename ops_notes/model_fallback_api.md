# モデル割当・フォールバック・出力上限（API）

## 適用条件
API 呼出が timeout / 429 / 空応答 / length で失敗する時、モデル割当を扱う時。

## 手順
- 割当の唯一の権威は `models.json:assignments`。表示・判定は `model_resolver.resolve_squad()`
  から取る。ベンダー判定は `models.json:vendors` 表（ID接頭辞から推定しない）。
- 429・timeout・応答不正・本文空のいずれでも `council._post_chat` が fallback_chain へ
  自動で切替える（`[hq] fallback: A -> B` が出る）。連鎖の定義元は `models.json:fallbacks`。
  実績のある連鎖: opus5→opus-4-8→fable5 / gpt-56-sol→gpt-56-terra / kimi-k3→kimi-k2-7-code。
- `models.json` の編集は PowerShell に JSON を貼らずエディタで行う。変更後は
  `python -c "import model_resolver as m; print(m.check_constraints())"` が `[]` を返すこと。

## 失敗と回避
- **出力が途中で切れる（finish_reason=length）時、まずクライアントが送る max_tokens を
  疑う**。`venice_client.chat` の既定 `max_tokens=8192`（19行目）が全モデル共通の上限で、
  モデル固有の停止不良ではない。モデル交換で直る問題と混同しない。
- `kimi-k3` は reasoning を completion に含むため max_tokens を食い切る。HQ3役など長い
  呼出は `max_tokens=16000` を明示する（既定2000だと設計案が length 切断され本文が空、
  フォールバック連鎖も全段同じ理由で失敗する）。
- `The read operation timed out`（kimi-k3 で頻発）は Venice 側。S36 以降は _post_chat が
  自動切替する。個別の再試行実装は不要。
- OpenAI 形式 tool_calls にドリフトするモデル（deepseek-v4-flash 等）は独自タグを
  装飾テキスト扱いして results count=0 で空転する。未解決の既知事象。自己申告の
  「モデルが悪い」をそのまま不具合登録しない。
- `non_projects` は models.json のトップレベルに置く。assignments 内に入れると
  `TypeError: unhashable type: 'list'`。
