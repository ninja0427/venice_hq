# 制作案件（ポスター・バナー・図版）の進め方

DESIGNER を持つ分隊だけが使う。読むのは制作案件を担当する時のみ。

## 見栄えを上げる（最重要・ここを外すと「やっつけ感」が出る）

平板に見える原因は3つ。**背景が無地／箱で並べただけ／文字の階層が弱い**。
機械チェック（done_check）に通っても、この3つを外すと商品ポスターとして失格。

### 5つの原則

1. **まず「1枚のクリーンな画像」として設計する。** 印刷/Web は問わない。結局は1枚の画像。
   - **イラストの情景は必須ではない。** むしろ広告画像はクリーンな面（無地／やさしい色／写真そのもの）
     ＋強い文字組みの方が今っぽく、美味しそうに見える。
   - イラストを使うなら**控えめに**。主役より目立たせない。「ポスターだから絵で埋める」は誤り。
   - 文字を置く余白を必ず確保する。
2. **主役は実物写真。大きく・美味しそうに。** 商品写真は COMPOSE で載せる（生成で描き起こさない）。
   - **「箱貼り」に見せない。** 手は3つ：
     (a) 写真をフルブリード／大きく敷いて、文字を上に載せる（食品広告の王道）。
     (b) 白背景の写真を白い面に置き、境目を消す。
     (c) **COMPOSE の写真に `"remove_bg": true` を付けて背景を抜く**（白背景で作った部品＝
        バッジ・リボン・葉・イラストに有効。濃さは `"bg_tolerance"`、既定32）。
        四隅から繋がる背景だけ抜き、**内側の白（家・文字）は残す**。
   - COMPOSE の文字で改行したい時は `\n` を入れてよい（改行として扱う。¥n にはならない）。
   - 料理は照り・質感が見える大きさで、画面の主役に。イラストで埋めない。
3. **飾りを2〜3点足す。** 丸バッジ（「〇〇100%」）・リボン・角丸の枠・下部の3アイコン帯。
   飾りも GEN_IMAGE で部品として単体で作る（`isolated on plain white background` → bg-remover で抜く）。
4. **文字は階層をはっきり。** 特大タイトル → 中サブ → 価格（太く色差） → 注記（小さく）。
   COMPOSE の文字サイズ・色・太さで差をつける。**全部同じ大きさにしない。**
5. **余白と整列。** 要素は中央線か三分割の線に載せる。ばらばらに置かない。

### 手順（情景背景 ＋ 飾り部品 ＋ 文字）

1. 情景の背景を GEN_IMAGE で作る（英語・text 禁止・上下どちらかに広い余白）。
2. 飾り部品（バッジ・リボン）を GEN_IMAGE で単体生成。必要なら bg-remover で抜く。
3. COMPOSE で重ねる：背景 → 商品写真 → 飾り → 文字（階層をつける）。
4. 戻り値の warnings を読み、はみ出し・解像度・重なりを直して出し直す。

### GEN_IMAGE プロンプトの格上げ（情景の型）

良い例：
```
warm pastoral poster illustration, a jersey cow on a green hill,
farmhouse, blue sky, soft watercolor, gentle morning light,
large clean empty band across the top for a title, no text, no letters
```
悪い例（今の失敗）：`cream background, minimal` … 情景が無く平板になる。

### 参考にする設計（考え方だけ借りる。ツールは違う）

- **baoyu-infographic**（Hermes 同梱）＝レイアウト21×スタイル21。世界観の作り方の宝庫。
  `[READ_FILE path="C:\Users\naked\AppData\Local\hermes\hermes-agent\skills\creative\baoyu-infographic\SKILL.md"]`
  スタイル本文は同フォルダ `references\styles\<style>.md`（例 storybook-watercolor / craft-handmade）。
- これらは「文字も画像に描く」前提。**日本語は必ず崩れる**ので、
  スタイル（配色・質感・情景）だけ真似し、**文字は COMPOSE で後載せ**する。

### スタイルの引き出し（背景を「らしく」する語彙）

GEN_IMAGE の背景が平凡になる時は、下の巨匠スタイルを1つ選び、英語プロンプトの
錨にする。「情景 ＋ スタイル名」で世界観が締まる。**画像に文字は入れない。**

暖かい・イラスト寄り（食品／牧場に合う）:
- `Alphonse Mucha style` … アールヌーヴォーの流れる曲線・装飾的（やさしい・上品）
- `Jules Cheret style` … 明るく楽しい色・動きのある構図（華やか）
- `Drew Struzan style` … 描き込んだ絵画調・郷愁のある光（あたたかい）
- `Jay Ryan style` … 手描きの素朴・温かい質感・シンプル（親しみ）

くっきり・モダン:
- `Saul Bass style` … 幾何学の最小構成・視覚的比喩
- `Milton Glaser style` … ポップで大胆・色の遊び
- `Olly Moss style` … 余白で語る・2色

作り込みの原則（qiaomu-mondo skill より）:
- 色数をしぼる（2〜4色）か、逆に情景を描き込む。**中途半端が一番平板。**
- 焦点はひとつ。要素をごちゃ混ぜにしない。
- 余白を活かす（目安：絵 30% ／ 文字 30% ／ 余白 40%）。
- 質感の語を足す：`halftone dot texture` / `paper grain` / `risograph`（安っぽさが消える）。

全文（20スタイル・余白技法・ジャンル別テンプレ）を読むなら:
`[READ_FILE path="C:\Developer\Projects\HQ\skills\_vendor\qiaomu-mondo-poster-design\SKILL.md"]`

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

- size は `poster`（3:4・A3向き）/ `poster_tall` / `poster_wide` / `square` / `banner` / `story`
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

## 大きさの指定（重要）

**`width` / `height` は使えない。** 新しいモデルでは廃止され、`flux-2-pro`
でも無視されて 1024x768 が返る（実測）。**比率で指定する。**

`size="..."` で選ぶ。CLI が比率へ翻訳する。

| size | 比率 | 実際に返る例 | 用途 |
|---|---|---|---|
| poster | 3:4 | 864x1152 | **A3・A4 に最も近い** |
| poster_tall | 2:3 | 832x1248 | もっと縦長 |
| poster_wide | 4:3 | — | 横位置 |
| square | 1:1 | 1024x1024 | SNS |
| banner | 16:9 | — | 横長バナー |
| story | 9:16 | 752x1344 | 縦長ストーリー |

## A3 で刷りたい時

A3 は 297x420mm、比率 1:1.414。使えるのは `poster`（3:4 = 1.333）が最も近い。

必要な画素数と、いまの手順で届くか。

| 用途 | 必要画素 | 届くか |
|---|---|---|
| A3 150dpi（掲示用） | 1754x2480 | **届く**（生成864x1152 → 4倍拡大で3456x4608） |
| A3 300dpi（印刷入稿） | 3508x4961 | **拡大だけでは不足** |

手順（150dpi まで）
1. `size="poster"` で生成する
2. 人間が拡大する。Venice の `image/upscale`（scale=4）を使う
3. 仕上げに A3 の比率へ切り取る（3:4 と 1:1.414 は少しずれるため）

**300dpi の入稿が要る案件**は、生成画像だけでは足りない。
背景・図版として使い、文字と罫線はベクタで別に載せる前提で設計すること。
この判断は人間に上げる。分隊が勝手に「足りている」と報告しない。

## 拡大について（未確定を含む）

- `image/upscale` は scale=2 と 4 が通る（実測）。
- ただし**元の縦横比を保たない**ことがある（896x1280 → 4096x3072 になった）。
  拡大後は必ずサイズを確認し、必要なら比率を直す。
- 拡大は分隊の担当外。人間が行う。
