# BNI AQUA チャプター運営ガイド

BNI AQUA に新しく入会されたメンバーが、**BNI を活用するための動き方** を Web ブラウザで一通り把握できる静的サイトです。BNI そのものの説明と、AQUA チャプター固有の情報（リーダー・Zoom・メンター・ハウスルール・期テーマ）を 11 タブにまとめています。

## ファイル構成

```
aqua-newmember/
├── index.html                              ← サイト本体（HTML骨格 + CSS + 描画ロジック）
├── content.js                              ← ★ 文章・リンク・名前など、書き換える対象はここだけ
├── README.md                               ← このファイル
├── PLAN.md                                 ← 実装の進捗チェックリスト
├── manual.txt                              ← PDF を pdftotext で抽出した全文（参照用、サイトには非含有）
└── チャプター運営マニュアル2026年3月版.pdf  ← 原本 PDF
```

## ローカルで開く

```sh
open /Users/arikawakoki/cowork/dev/aqua-newmember/index.html
```

または `index.html` をダブルクリック。サーバー不要です。

## 11 タブの構成

| # | タブ | 主な内容 |
|---|---|---|
| 1 | BNIとは | Mission / Vision / 規模 / VCP プロセス / 大阪シティセントラルリージョン / **AQUA とは** / 11 期テーマ / **AQUA ハウスルール** |
| 2 | AQUA | AQUA 活用ガイド（1to1 / トレーニング / イベント / ビジター招待）/ 信頼関係（倫理規定 / 時間 / トラブル対処） |
| 3 | 準備する | 初回定例会前にやること（プロフィール登録 / 25 秒プレゼン原稿 / ツール登録 / 当日準備） |
| 4 | 定例会 | Zoom 入室情報 / 24 項目アジェンダ / リンク集 |
| 5 | 定例会後 | 期待値設定 / 1to1 準備 / トレーニング体系 / イベント / 1 年スケジュール |
| 6 | メンター | パスポートプログラム / メンター 10 名表 / 初回チェックリスト |
| 7 | 貢献・スコア | スコア表（カラー判定）/ ビジター招待 3 ステップ / 実務フロー |
| 8 | 入会手続き | 費用 / BNI コネクトでの申込 6 ステップ |
| 9 | その他 | 欠席対応 / リファーラルが出ないとき / ビジネスにつながらないとき / 退会・更新 / リンク集 |
| 10 | 用語集 | 1to1 / TYFCB / PALMS / VCP など 26 項目 |
| 11 | FAQ | 入会・出席・プレゼン・スコア・ツールに関する Q&A |

## 内容を更新したいとき

`content.js` の中だけを編集すれば、`index.html` / CSS には触らずに更新できます。

### よく書き換える箇所（ファイル冒頭にまとめてあります）

```js
window.SITE_INFO         // タイトル・期名・テーマ・フッター
window.LEADERS           // リーダーシップチーム（プレジデント / VP / 書記兼会計）
window.ZOOM_INFO         // Zoom ID・パスコード・URL・入室時刻・表示名
window.LINKS             // 各種 URL（コネクト / Builder / メンバーリスト / 各種フォーム）
window.MENTORS           // パスポートプログラム担当者 10 名
window.MENTOR_COORDINATOR // メンターコーディネーター名
window.FEES              // 登録費・年会費
window.THEME             // 期テーマのタイトルと説明
```

例：期が変わってリーダーが入れ替わったら `LEADERS` の `name` だけ書き換えれば、ヘッダー右側と用語集に自動反映されます。

### セクション（タブ）の追加・並べ替え

`window.SECTIONS = [ ... ]` の配列順がそのままタブの並びになります。各セクションは：

```js
{
  id: "prepare",          // 半角英数字。URL の #anchor、CSS、ジャンプリンクで使う
  navLabel: "準備する",   // タブのラベル
  eyebrow: "03 — Prepare", // h2 の上に表示される小さな英文ラベル
  title: "準備する — 初回定例会前までにやること",
  blocks: [ ... ]         // 本文ブロックの配列
}
```

### ブロック種別

| type           | 用途                          | 主なフィールド |
|----------------|-------------------------------|---|
| `para`         | 段落                          | `html` |
| `h3` / `h4`    | 見出し                        | `text`, `sub: true` で赤の小見出しに |
| `divider`      | 区切り線                      | — |
| `box`          | 強調ボックス                  | `variant` (`red`/`gray`/`warn`), `label`, `html` |
| `list`         | ▶印リスト                     | `items: [文字列]` |
| `steps`        | ラベル付きステップ            | `items: [{label, title, desc, list?, link?, variant?}]` |
| `grid`         | カードグリッド                | `cols: 2/3/4`, `cells: [{num, title, body, accent?}]` |
| `table`        | 通常表                        | `head: [...]`, `rows: [[...]]` |
| `score-table`  | カラー判定スコア表            | `head`, `rows`（セルを `{v, c}` で色指定）, `centerCols: [1,2,...]` |
| `agenda`       | 時間軸の進行表                | `rows: [{time, title, desc, key?, badge?, breakoutLink?}]` |
| `vcp`          | VCP プロセス図                | `steps: [{letter, title, desc, period}]` |
| `theme`        | 期テーマ表示                  | （`window.THEME` を自動表示） |
| `zoom`         | Zoom 入室情報ボックス         | （`window.ZOOM_INFO`/`LINKS.zoom` を自動表示） |
| `checklist`    | A/B/C... のチェックリスト     | `items: [{label, title, desc, linkUrl?, linkText?}]` |
| `tiers`        | カテゴリー × ピル群             | `rows: [{label, items, variant: 'urg'/'gray'/'dark'}]` |
| `links`        | アイコン付きリンクボタン群    | `groups: [{heading, items: [{icon, url, title, sub}]}]` |
| `mentor-table` | メンター 10 名表              | （`window.MENTORS` を自動表示） |
| `glossary`     | 用語集                        | `items: [{term, def}]` |
| `faq`          | カテゴリー付き FAQ            | `items: [{cat, q, a}]` |

### 文中で使える変数置換

`content.js` の文字列中に書くと、レンダリング時に自動で値が入ります。

| 書き方                       | 置換される値 |
|-------------------------------|---|
| `${LINK:zoom}` 等             | `window.LINKS.zoom` の URL |
| `${ZOOM:meetingId}` 等        | `window.ZOOM_INFO.meetingId` |
| `${FEE:registration}` 等      | `window.FEES.registration` |
| `${MENTOR_COORDINATOR}`       | `window.MENTOR_COORDINATOR.name` |

例：
```js
{ type: "box", variant: "red", html: "Zoom ID は ${ZOOM:meetingId} です" }
```

### よく使う書き方

- 太字 → `<strong>テキスト</strong>`
- 赤い強調（タイトル中など）→ `<em>テキスト</em>`
- 改行 → `<br>`
- リンク → `<a href="..." target="_blank" rel="noopener">表示テキスト</a>`
- シングルクォートを文字として → `\'`

### 動作確認

ブラウザで `index.html` を開き、

- 11 タブが切り替わる（Sticky でスクロール時もタブが固定される）
- ヘッダー右側にリーダーシップチーム名が表示される
- 定例会タブで Zoom ID / パスコード / 入室リンクが表示される
- メンタータブで 10 名のメンターが表示される
- スコア表のセルに色がついている（赤・黄・緑）
- スマホ幅でも崩れない
- ブラウザのコンソールにエラーが出ていない

## ホスティング

静的ファイルだけなので、任意の静的ホスティング（Netlify / Vercel / GitHub Pages / Cloudflare Pages 等）にそのままデプロイできます。

## 出典・著作権

本サイトの内容は、BNI 公式『チャプター運営マニュアル 2026 年 3 月版』および AQUA チャプター内で運用されているガイド（v6）を参考に再構成しています。BNI® および BNI のロゴは BNI Global, LLC の登録商標です。
