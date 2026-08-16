# 制作案件（ポスター・バナー・図版）の進め方

DESIGNER を持つ分隊だけが使う。読むのは制作案件を担当する時のみ。

## 役割分担

| 誰が | 何をするか |
|---|---|
| LEAD | 依頼を分解し、誰に何をさせるか決める |
| SCOUT | 参考例・競合・レギュレーションを調べる |
| CODER | キャッチコピー・本文・原稿を書く（テキストモデル） |
| DESIGNER | 画像を作る（GEN_IMAGE） |
| CHECKER | 誤字・数値・レギュレーション違反を照合 |
| SCRIBE | 現況を記録する |

## 画像を作る

```
[GEN_IMAGE name="案A" size="poster"]
英語で、描くものを具体的に書く
[/GEN_IMAGE]
```

- size は `poster`（896x1280）/ `poster_wide` / `square` / `banner` / `a4`
- 1回の応答で最大3枚
- 保存先は案件配下の `_assets\`。CLI が決めるのでパスは書かない

## 絶対に守ること

- **日本語の文字を画像に描かせない。** 必ず崩れる。
  文字が入る場所は「余白」として空け、文字は後段で載せる。
  例: `large empty space at top for a title`
- **プロンプトは英語で書く。** 日本語だと精度が落ちる。
- **分隊は自分が作った画像を見られない。** 出来を自己評価しない。
  「良い仕上がり」と書かない。作った事実とパスだけを報告する。
- 同じプロンプトで作り直さない。変えるなら、どこを変えたか1行で書く。

## プロンプトの型

要素を順に並べる。

1. 何を（minimal event poster / product banner）
2. 構図（large empty space at top, centered subject, rule of thirds）
3. 配色（deep navy background, cyan accents）
4. 質感・様式（flat vector, high contrast, film grain, 3D render）
5. 除外（negative に入れる。text, watermark, logo など）

良い例:
```
minimal event poster, large empty space at top for a title,
deep navy background, cyan geometric accents, flat vector,
high contrast, generous margins
```

悪い例: `かっこいいポスター`（毎回違うものが出る）

## 進め方の順序

1. 依頼の確認 … 用途・寸法・締切・使用媒体・NG事項を洗い出す
2. 調査 … SCOUT に参考例を集めさせる
3. コピー … CODER に案を3つ書かせる（短い順に並べる）
4. 画像 … DESIGNER に案を3つ作らせる。方向性を変えて作る
5. 照合 … CHECKER に誤字・数値・NG事項を確認させる
6. 提出 … 人間が目視で選ぶ。分隊は優劣を断定しない

## 人間に返すもの

- 画像のパス（複数案）
- コピー案（複数）
- 各案が狙ったこと（1行ずつ）
- 未確定な点（寸法・締切・素材の権利など）

## 画像モデルの使い分け

| 用途 | モデル |
|---|---|
| 標準・高品質 | flux-2-pro |
| 文字やロゴを含む構図 | recraft-v4-pro / ideogram-v4 |
| 素早い試作 | z-image-turbo |
| 部分の描き直し | nano-banana-pro |
| 背景の除去 | bria-bg-remover |

`model="..."` で切り替えられる。既定は編成の DESIGNER。

## 大きさの制約

画像 API は**縦横とも 1280px が上限**（実測。超えると 400 エラー）。
印刷用に大きくしたい場合は、この比率で作ってから後段で拡大する。
