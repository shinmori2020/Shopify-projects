# 自作レビューセクション 仕様書 — `komorebi-reviews-native`

Komorebi（sin-shop-dev）の「お客様の声（自作版）」セクションの仕様。
アプリ（Judge.me）が集めたレビューを、**自作Liquidで独自デザイン表示**する実装。

- ファイル：`sin-shop-dev/sections/komorebi-reviews-native.liquid`
- セクション名（エディタ表示）：**レビュー（自作版）**
- 配置：トップページ（`templates/index.json`／配置はテーマエディタで実施）

---

## 1. コンセプト
**収集はアプリ／表示は自作** の「いいとこ取り」。

```
顧客が投稿 → Judge.me が収集・公開
        ↓ Shopify メタフィールドへ書き出し
自作Liquid が読み取り → 独自デザインで表示
```

- メリット：デザイン完全自由・月額なしで表示・技術力アピール
- 注意：表示は**メタフィールド依存**（後述の同期ラグあり）

---

## 2. データ元
各商品のメタフィールドを Liquid で読む。

| メタフィールド | 用途 |
|---|---|
| `judgeme.review_widget_data`（json） | 個別レビュー（本文・★・名前・日付）＋ `number_of_reviews` / `average_rating` |
| （標準）`reviews.rating` / `reviews.rating_count` | 移植性の高い平均・件数（補助） |

- セクションは**対象コレクション（未指定なら全商品）をループ**してレビューを集約。
- 評価サマリーは**全商品から加重平均**で算出。

---

## 3. 表示・レイアウト
- **表示スタイル**：`carousel`（全幅・1カードずつ自動送り）/ `grid` を切替
- **カルーセル**
  - 画面端〜端の**全幅（フルブリード）**
  - **1カードずつ「停止→スライド」**で自動送り（カード枚数に応じた `@keyframes` を Liquid が生成）
  - **表示枚数 3〜6（既定5）**。カード幅 = `画面幅 ÷ 枚数`（奇数なら中央に1枚）
  - マウスホバーで一時停止／`prefers-reduced-motion` でアニメ無効
  - シームレスループのためカードを2セット複製し `translateX(-50%)`
- **グリッド**：列数指定（PC）。タブレット2列・スマホ1列に自動調整

---

## 4. カードの中身
- **★星評価**：大きさ・色を設定で変更可
- **本文**：指定行数（既定3行）で `line-clamp` → 超過は「…」省略
- **投稿者名・商品名**：1行で省略（`text-overflow: ellipsis`／折り返さない）。商品名はリンク
- **日付**：`YYYY/MM/DD`

---

## 5. 見出し下の評価サマリー
- 全商品の `average_rating × number_of_reviews` を集計した**加重平均★ と 総件数**
- 半端な点（例 4.57）も、グレー★の上に**色付き★を幅 `平均/5×100%` でクリップ**して**部分塗り**表示
- 表示 ON/OFF 可

---

## 6. 全文モーダル（クリック時）
- カードクリック／キーボード（Enter/Space）で**フェードイン表示**
- 内容：★・**本文全文**・名前・商品名・日付
- **「この商品を見る →」** ボタン → 商品ページ（購入導線）
- 閉じる：**×／背景（暗幕）クリック／ESC**
- 開いている間はカルーセルの自動送りを**一時停止**
- スマホは**下から出るシート風**
- 実装：`<body>` 直下へ移動して暗幕を全画面化。`is-open` クラス＋opacity/transform でフェード

---

## 7. 設定項目（テーマエディタ）
| グループ | 設定 | 内容 |
|---|---|---|
| 基本 | `heading` / `subheading` | 見出し・サブ見出し |
| 表示 | `layout` | carousel / grid |
| 表示 | `visible` | 1行の表示枚数（PC・3〜6） |
| 表示 | `speed` | カルーセル一周の秒数 |
| 表示 | `columns` | グリッド時の列数 |
| 表示 | `body_lines` | 本文の表示行数（超過は…） |
| デザイン | `star_color` | 星・ボタンの色 |
| デザイン | `star_size` | カードの星の大きさ(px) |
| デザイン | `card_bg` / `card_border` / `card_radius` | カード背景・枠線・角丸 |
| デザイン | `color_scheme` | 背景の配色スキーム |
| 全文モーダル | `enable_modal` | モーダルON/OFF |
| 全文モーダル | `read_more_label` | 「続きを読む」文言 |
| 全文モーダル | `product_button_label` | 商品ボタン文言 |
| 項目の表示 | `show_summary` | 評価サマリー表示 |
| 項目の表示 | `show_product` / `show_date` | 商品名・日付の表示 |
| 項目の表示 | `min_rating` | 表示する最低★ |
| 項目の表示 | `max_reviews` | 表示するレビュー数 |
| 項目の表示 | `collection` | 対象コレクション（未指定＝全商品） |
| 余白 | `padding_top` / `padding_bottom` | 上下余白 |

---

## 8. 技術メモ・既知の注意点
- **重要CSSはインライン `<style>` で配信**：Dawn の `{% stylesheet %}` が一部ルール（modal-overlay/box・line-clamp 等）を**配信から落とす**不具合があったため、モーダル・行数省略・サマリー・星サイズは**インライン化して確実化**。
- **Liquid の `if` 条件内でフィルター（`|`）は使用不可** → 事前に `assign` で変数化する（例：`cards_stripped`）。
- **メタフィールド同期ラグ**：レビューを削除→再インポートした直後は、アプリ版（API）は即時だが、自作版（メタフィールド読み）は**数十秒〜数分**反映が遅れる。Admin APIで `judgeme.review_widget_data` の `number_of_reviews` を確認すると同期状況が分かる。
- **CSS変数**：色・幅・行数・星サイズはルート要素の inline style で CSS変数として出力し、設定変更が即反映。
- カルーセルは `width: max-content` のトラックを `translateX` でアニメ。カードは固定幅＋右マージン。

---

## 9. 次回TODO
- [ ] 見出し下サマリー★・モーダル内★も大きくする（カード★は対応済み）
- [ ] （任意）「もっと見る」ボタンの出し分け表示の微調整

---

## 10. 実装用プロンプト（再利用）
別案件でも同じセクションを作れるよう、AIに渡す指示文をそのまま記載。
（前提：Shopify Dawnベースのテーマ＋Judge.me導入済みストア）

```text
Shopify Dawn テーマ用に、Judge.me のレビューを「自作デザイン」で表示するカスタムセクション
（sections/komorebi-reviews-native.liquid 相当）を1ファイルで作成してください。

# データ元
- 各商品のメタフィールド `judgeme.review_widget_data`（type: json）を Liquid で読む。
  - 含まれるキー: reviews[]（各 body / rating / reviewer_name / created_at）, number_of_reviews, average_rating
- 対象コレクション設定（未指定なら collections.all.products）をループしてレビューを集約。
- 表示件数の上限（max_reviews）と最低★（min_rating）で絞り込む。

# 表示・レイアウト
- 表示スタイルを carousel / grid で切替（schemaのselect）。
- carousel:
  - 画面端〜端の全幅（フルブリード）。Dawnの color-{scheme} gradient 帯の中で page-width を使わず全幅にする。
  - 「1カードずつ停止→スライド」で自動送り。カード枚数 n に応じて @keyframes を Liquid で生成
    （各カードは slot=100/n% の時間、前半 pause(約72%)は静止、後半でスライド。0→translateX(-50%) を n 等分）。
  - カードは2セット複製して translateX(-50%) でシームレスループ。
  - 1行の表示枚数 visible(3〜6)。カード幅 = (100/visible)vw - 右マージン（奇数なら中央に1枚）。
  - ホバーで一時停止、prefers-reduced-motion でアニメ無効。
- grid: 列数指定、タブレット2列・スマホ1列。

# カード
- ★星（is-on を色付け、サイズは設定 star_size px）。
- 本文は body_lines 行で -webkit-line-clamp して「…」省略。
- 投稿者名・商品名を1行に省略（white-space:nowrap; overflow:hidden; text-overflow:ellipsis）。商品名はリンク。
- 日付 YYYY/MM/DD。

# 見出し下の評価サマリー
- 全商品の average_rating × number_of_reviews を集計した加重平均★と総件数を表示。
- 半端な点も「グレー★5つ」の上に「色付き★5つ」を width:(平均/5*100)% でクリップして部分塗り。

# 全文モーダル
- カードクリック/Enter/Space でフェードイン表示。★・本文全文・名前・商品名・日付・「この商品を見る→」ボタン（商品URL）。
- 閉じる: ×／背景クリック／ESC。開いている間はカルーセルを一時停止。
- モーダルは JS で document.body 直下へ移動（親のstacking/transform影響回避）。is-open クラス＋opacity/transformでフェード。
- スマホは下から出るシート風。

# 設定（schema settings）
heading, subheading, layout(select), visible(range3-6), speed(range秒),
columns(range), body_lines(range2-6), star_color(color), star_size(range px),
card_bg(color), card_border(color), card_radius(range), color_scheme,
enable_modal(checkbox), read_more_label(text), product_button_label(text),
show_summary(checkbox), show_product(checkbox), show_date(checkbox),
min_rating(range1-5), max_reviews(range), collection(collection),
padding_top/padding_bottom(range)。presets も用意。

# 技術的な必須ルール（重要）
- Dawn の {% stylesheet %} は一部CSSルール（modal-overlay/box, line-clamp 等）を配信から落とすことがある。
  → モーダル・行数省略・評価サマリー・星サイズ等の「重要CSS」は section 内のインライン <style> に書いて確実に配信する。
- Liquid の {% if %} 条件内でフィルター（|）は使えない。必ず {% assign %} で変数化してから比較する。
- 色・幅・行数・星サイズはルート要素の inline style に CSS変数（--nrev-*）として出力し、設定変更を即反映。
- セクションは複数設置されても衝突しないよう、id やキーフレーム名に section.id を含める。
- json型メタフィールドは .value で配列/オブジェクトとして for で回せる。
- 文字列は escape、data属性経由でモーダルへ渡しJSで textContent に入れる（XSS対策）。

出力は sections/komorebi-reviews-native.liquid の完全な1ファイル（liquid + 必要なinline style/script + {% schema %}）。
```

> このプロンプトは「Judge.me前提」。別アプリやメタオブジェクト版に流用する場合は「データ元」セクションを差し替える。
