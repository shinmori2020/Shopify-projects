# WORKLOG（作業フロー記録）— Komorebi / sin-shop-dev

このファイルは、Shopifyストア「Komorebi」(sin-shop-dev) の作業フローを記録するログです。
**新しい作業を始めたら、下の「作業ログ」に1ブロック追記**していきます（着手前に予定、完了後に結果）。

- 関連スキル：`shopify-capabilities`（できること・手段・前提・難易度）/ `shopify-theme-workflow`（安全な編集手順）
- 方針：ローカルサーバー常時起動はしない／編集→下書きpush→確認→Git保存／公開は承認後

---

## 進行中のゴール：SHOPサイト機能 ①②③ の作り込み

機能を3段階で順に整備する（詳細は `shopify-capabilities` 参照）。

### ① 最低限必須（無いと成立しない）
- [x] 商品ページ・カート・チェックアウト（Dawn標準）
- [x] カテゴリー(コレクション)・検索（Dawn標準）
- [x] 商品データ・在庫・購入可能化
- [ ] 送料・配送設定の確認
- [ ] 🇯🇵 特商法表記・プライバシー/返品/利用規約・会社情報・問い合わせページ
- [ ] 決済方法の確認

### ② あって当然（標準的に期待される）
- [x] 会員/マイページ・注文/配送メール（Dawn標準）
- [x] 絞り込み・並べ替え・関連商品（Dawn標準）
- [x] アナウンスバー（要文言調整）
- [ ] クーポン/割引の用意
- [ ] SEO基本（タイトル/メタ/OGP）整備
- [ ] FAQ・ニュースレター登録の整備

### ③ あれば嬉しい（差別化/売上UP・優先順）
- [x] 商品レビュー（🧩 Judge.me 無料プラン導入済 2026-06-25）
- [ ] かご落ちメール（🧩）
- [ ] 予測検索＋高度な絞り込み（🧩 Search & Discovery）
- [ ] アップセル/クロスセル/バンドル（🧩🛠）
- [ ] サイズガイド／ルックブック／UGC(Instagram)（💬自作/🧩）※アパレル相性◎
- [ ] 入荷お知らせ／ウィッシュリスト（🧩）
- [ ] ギフト対応（ラッピング/メッセージ/配送日指定）（🧩🛠）
- [ ] ポイント/会員特典／サブスク（🧩）
- [ ] 決済の選択肢拡充（Shop Pay/分割/コンビニ/後払い）（👤🇯🇵）
- [ ] 多言語・多通貨（💬💎 越境時）

---

## アプリ導入チェックリスト（無料で実装できるもの）

方針：**公式アプリ優先＋定番の無料枠**を厳選。⭐=おすすめ優先。入れすぎは表示速度・管理が重くなるので取捨選択する。

### 導入済み
- [x] Translate & Adapt（多言語・公式）※言語設定で使用
- [x] ⭐Judge.me（商品レビュー・無料プラン）2026-06-25

### 公式アプリ（完全無料・優先）
- [ ] ⭐Shopify Search & Discovery（予測検索・絞り込み・関連商品）→ ③予測検索/絞り込み
- [ ] ⭐Shopify Inbox（接客チャット）
- [ ] Shopify Email（メルマガ配信／無料枠 月1万通）→ ②ニュースレター
- [ ] Shopify Forms（登録ポップアップ・リスト獲得）→ ②ニュースレター
- [ ] Shopify Bundles（セット販売・アップセル）→ ③バンドル
- [ ] Shopify Subscriptions（定期購入）※アパレルでは任意

### 定番サードパーティ（無料枠あり）
- [ ] ⭐Smile.io（ポイント/会員/紹介）→ ③ポイント
- [ ] Instafeed（Instagram連携ギャラリー・UGC）→ ③UGC
- [ ] Wishlist系（お気に入り）※無料のものを選定 → ③ウィッシュリスト
- [ ] Back in Stock（入荷お知らせ）※無料枠 → ③入荷通知

---

## 案件研究タスク（クリエイターグッズEC）— 中・上級実装（低優先）

きっかけ：クリエイターグッズEC案件（Crazy Raccoon系）の仕様研究で洗い出した未実装機能。**簡潔な実装が一通り終わってから着手**する。

### ★★ 中級（アプリ/標準機能で対応・比較的軽い）
- [ ] 予測検索＋高度な絞り込み（Shopify Search & Discovery／無料）※[アプリ導入チェックリスト]と連動
- [ ] Instagram連携（Instafeed等）※同上
- [ ] お気に入り（ウィッシュリスト・アプリ）※同上
- [ ] NEWS（ブログ）… Shopify標準のブログ機能で対応
- [ ] ヒーローのスライダー化（Dawnのスライドショー）
- [ ] ヘッダー/フッターの導線追加（CREATOR/EVENT/FANCLUB等のメニュー）

### ★★★ 上級（独自システム・重い）
難所の本質：①箱が無い→**メタオブジェクト**で自作／②**逆引き**（このcreator/eventの商品一覧）がLiquidで苦手／③**時間制御・課金**は標準に無い。アプリで肩代わりできる部分は「アプリ＋デザインは自作Liquid」のハイブリッドで。

- [ ] ★★★ **CREATOR**（一覧・詳細）… メタオブジェクト or **vendorフィールド＋自動コレクション**で逆引き解決／プロフィール・SNS・商品数
- [ ] ★★★ **EVENT**（一覧・詳細・販売期間）… メタオブジェクト＋**スケジュールアプリ(Launchpad/無料)**で販売期間の自動オン/オフ
- [ ] ★★★ **FANCLUB**（プラン/加入/会員特典）… **会員/サブスクアプリ必須**（継続課金＋会員限定の出し分け）

> Komorebi本体に入れるか、別デモ（クリエイターグッズEC）として作るかは着手時に判断。判断基準＝「物販の安定性ならShopify／独自構造の自由ならWP」。

---

## 作業ログ（新しい順に追記）

### 2026-06-26 レビューをトップへ＆「自作Liquid版」を併設（アプリvs自作）
- 目的：トップ「お客様の声」を本物レビュー連動に。さらに独自性/デザイン性のため自作版も作って比較
- 手段：Judge.me Cards Carousel（アプリ）＋ 自作セクション（メタフィールド読み）＋ カスタムCSS
- 実装：
  - トップの固定「お客様の声」→ Judge.me **Reviews Carousel** に差し替え（本物レビューが自動表示）
  - `assets/komorebi-custom.css` に **カルーセル調和CSS**（`--card-color`白化/角丸6px/影なし/page-width幅）を追記
  - **自作セクション `sections/komorebi-reviews-native.liquid` 新規**：`product.metafields.judgeme.review_widget_data`(json) と標準 `reviews.rating` を Liquid で読み、独自デザイン表示（全商品ループ→最新N件・列数・最低★を設定可）
  - トップのアプリ版カルーセルの**下**に自作版を配置（見比べ用）
- 学び：Judge.meは収集レビューを**Shopifyメタフィールドに書き出す**（`judgeme.review_widget_data`=全文JSON / `reviews.rating`=平均 / `reviews.rating_count`=件数）。→ **収集=アプリ / 表示=自作Liquid** の使い分けが可能。新レビューは自動反映（同期+キャッシュで数十秒〜数分の遅延／メタフィールドは件数上限あり）
- 反映：CSS・新セクションは `--only` で claude-draft(live) に push 済み。index.json はエディタ編集分を壊さないため触らず、配置はエディタで実施
- コミット：未（次でまとめて）／レビューCSV・画像URL取得はGit対象外

### 2026-06-26 テーマを claude-draft に統一・公開(live化)
- 目的：本番テーマを最新の `claude-draft` に一本化（test-dataとの分裂を解消）
- 背景：公開中=test-data（旧・作り込み無し）、作業=claude-draft（最新・全部入り）に分かれており、Judge.meがtest-dataに入って表示されない事故が発生
- 手順：オンラインストア→テーマ→`claude-draft`を「公開する」
- 結果：**`claude-draft` が本番(live)に。`test-data` は下書き（バックアップ）に降格**。以後アプリ/編集はclaude-draftに統一
- 次：本番ページ確認／レビューのデザイン仕上げ＋★バッジ／英語残り(You may also like等)の和訳／無料アプリ追加

### 2026-06-25 アプリ①：Judge.me（商品レビュー）導入・実装
- 目的：「お客様の声」を本物の投稿レビューと連動／商品ページにレビュー機能を追加
- 手段：Shopify App Store からインストール（無料プラン）＋テーマ組込（claude-draft）＋CSVインポート
- 手順：
  1. Judge.me インストール（無料プラン）。ウィジェット言語=日本語／ブランドカラー #1F7A5A
  2. デモレビュー14件をCSVで一括インポート（`komorebi-products.csv`とは別の `komorebi-reviews.csv`／商品ハンドルで紐付け）
  3. ⚠️テーマ不一致が発覚：公開中=`test-data`、作業中=`claude-draft`。Judge.meは当初 test-data に入り claude-draft では非表示だった
  4. `claude-draft` 側で App embed「Judge.me」をON＋商品ページに Review Widget を**独立セクション**で追加（商品情報の右列だと狭いため）
  5. 商品ページ末尾の英語仮セクション（テキスト付き画像/マルチカラム/Button label）を削除
- 結果：商品ページに日本語レビュー表示（例 wool-knit ★4.5/2件）。緑の星でブランド統一
- 残：★星評価バッジを商品名下に追加／レビューのデザイン仕上げ（A案ミニマル）／関連商品見出し「You may also like」等の和訳
- ★重要：**最終的にテーマを `claude-draft` に統一して公開（live化）する必要あり**（現在 live は test-data の古い版）
- コミット：未（レビューCSVはGit対象外でOK）


### 2026-06-24 トップを王道構成に再構成（USP帯・特集・お客様の声を追加）
- 目的：ECトップの王道フロー（世界観→商品→探す→共感→信頼→行動）に整える
- 手段：自作セクション2つ＋Dawn標準image-with-text＋Admin API(特集画像)
- 追加/変更：
  - `sections/komorebi-usp.liquid`（USP帯・SVGアイコン：送料無料/天然素材/30日返品）新規
  - `sections/komorebi-reviews.liquid`（お客様の声・星評価＋引用＋名前）新規
  - 特集：Dawn `image-with-text`＋画像 komorebi-feature.jpg(Burst woman-in-summer-fashion)
  - 並び順：ヒーロー→USP帯→おすすめ商品→カテゴリー→特集→Komorebiについて→約束→お客様の声
- 注意：カスタムセクションのblock設定 text型は**default必須（空文字NG）**。USPのsubtextでpushエラー→default入れて解消
- コミット：未（次でまとめて）


### 2026-06-24 ストアの日本語・日本仕様化（言語/通貨/国/メニュー）
- 目的：ヘッダー/フッター等を日本語・日本向けに
- 手段：ストア設定（言語/通貨/マーケット）＋テーマ編集（footer-group.json）＋Admin API（メニュー名）
- 変更：
  - 言語：日本語を追加・公開し**ドメインの既定を日本語**に（テーマUI文言が日本語化＝Dawnのja.json）
  - 通貨：基準通貨を**JPY**に（価格が$4,900→¥4,900に是正）。単位=メートル法/kg、TZ=東京、バックアップ地域=日本
  - マーケット：**日本を作成・アクティブ**、米国/カナダを下書き（無効）→国/通貨セレクター解消
  - フッター文言（footer-group.json）：ショップ/インフォメーション/Komorebiについて/ニュースレターを購読する に
  - メニュー名（Admin API menuUpdate, 要 read/write_online_store_navigation スコープ追加）：ホーム/商品一覧/お問い合わせ/検索/プライバシーの選択
- 注意：言語/通貨/マーケット/メニューは**ストア側設定でGit対象外**。footer-group.json は**テーマ＝要Gitコミット（未）**
- 次：（任意）ヘッダー/フッターにカテゴリーリンク追加（nav write権限あり）／①-2 必須ページ


### テンプレート（コピーして使う）
```
### YYYY-MM-DD ＜タスク名＞
- 目的：
- 手段：（テーマ編集 / Admin API / CLI など）
- 手順：
  1.
  2.
- 結果：
- コミット：（ハッシュ / メッセージ）
- 次への申し送り：
```

---

### 2026-06-23 ①改善 1-1c：約束セクションを自作（SVGアイコン）＋ヒーロー見出し改行制御
- 目的：依頼対応（写真→SVGアイコン／左寄せ＋増量／大見出しを改行させない）
- 手段：カスタムセクション自作（`sections/komorebi-promise.liquid`）＋CSSアセット＋theme.liquidでCSS読込
- 変更：
  - `sections/komorebi-promise.liquid` 新規作成：インラインSVG（葉/はさみ/ハンガー）＋左寄せ＋本文増量。schema/blocks/presets付き
  - index.json：multicolumn → komorebi_promise に差し替え（並び順も）。色scheme-2の薄グレー帯
  - `assets/komorebi-custom.css` 新規：`@media(min-width:750px){.banner__heading{white-space:nowrap}}`（大見出しをPCで1行に）。`layout/theme.liquid` の </head> 直前で読込
- 結果：約束が自作アイコンセクションに。ヒーロー大見出しがPCで改行しない
- theme check：新規エラーなし（既存Dawnの2件のみ）
- コミット：未（次でまとめて）

### 2026-06-23 ①改善 1-1b：背景白化・ヒーロー強化・Komorebiについて充実・約束セクション作り込み
- 目的：トップの完成度を上げる（依頼に沿った微調整）
- 手段：テーマ編集（settings_data.json・index.json）＋Admin API（約束用画像3枚アップロード）
- 変更：
  - 背景：scheme-1 を #FDF2E9(クリーム)→ #FFFFFF(白)
  - ヒーロー：文言をブランド訴求に刷新／配置を bottom-center→ middle-left（左揃え・上下中央）／ボタン「コレクションを見る」
  - Komorebiについて：本文を2段落に増量＋上下余白 40/0→72/72
  - Komorebiの約束：各列に画像追加(komorebi-promise-material/craft/design)・背景を薄グレー(scheme-2)・画像比率square・余白56
- 結果：トップが白基調＋写真入りで“作り込まれた”印象に
- コミット：未（次でまとめて）
- メモ：Burst画像は `?width` で壊れ画像(約257KB)を返すスラッグがある→アップロード後 fileStatus=READY と実寸で要確認

### 2026-06-23 ①改善 1-1：ホームの仮コンテンツ整備＋アナウンスバー
- 目的：英語の仮コンテンツを排除し、Komorebiらしい“完成した”トップにする
- 手段：テーマ編集（`sections/header-group.json`・`templates/index.json`）→下書きpush
- 手順：
  1. アナウンスバー文言を変更
  2. リッチテキスト=「Komorebiについて」＋ブランドストーリーに
  3. 「Featured products」→「おすすめ商品」
  4. 複数列=「Komorebiの約束」＋3つの特徴に
  5. 仮セクション（コラージュ/動画）を削除し、並び順を整理
- 結果：トップが ヒーロー→Komorebiについて→おすすめ商品→カテゴリーから探す→Komorebiの約束 の流れに
- コミット：未（次でまとめてコミット予定）
- 次への申し送り：1-2 必須ページ（特商法/ポリシー/About/問い合わせ）へ

### 2026-06-23 トップに「カテゴリーから探す」セクション＋コレクション整備
- 目的：商品をカテゴリーで探せる導線を作る
- 手段：Admin API（コレクション作成・公開・タグ付与）＋テーマ編集（collection-listセクション）
- 手順：
  1. タグ自動分類でコレクション3つ作成（トップス/ボトムス/雑貨）
  2. オンラインストアへ公開（publishablePublish）／最初のTシャツにタグ付与
  3. `templates/index.json` に collection-list セクション追加 → 下書きpush
- 結果：カテゴリー導線が表示・機能（公開設定で仮表示→解消）
- コミット：d6d011e 他
- 次への申し送り：ヘッダーメニューにもカテゴリー追加すると導線が完成

### 2026-06-22 ヒーロー刷新・在庫・画像比率・Admin APIトークン
- 目的：トップの主役をKomorebi仕様にし、購入可能な状態に
- 手段：Admin API（画像アップロード/在庫）＋テーマ編集（ヒーロー/画像比率）
- 手順：レガシーカスタムアプリ作成→トークン取得→画像fileCreate→index.json編集→push
- 結果：ヒーロー刷新、全商品購入可能、画像比率統一
- コミット：4f85337 他
- 次への申し送り：トークンは `C:\Users\kiyos_000\.komorebi-admin-token`（Git管理外）

### 2026-06-20 商品カタログ構築（Komorebiアパレル）
- 目的：架空ブランドの商品をそろえる
- 手段：管理画面（手動1点）＋CSVインポート（8点）＋Burst画像
- 結果：商品9点・サイズ展開・画像・販売元Komorebi
- 次への申し送り：Burst画像は `?width=2048〜2400`（25MP制限回避）

### 〜2026-06-18 環境構築・Level0/1
- Dawn取得・Git/GitHub・CLAUDE.md・theme dev・MCP・スキル導入・色変更（クリーム×緑）
- 詳細は memory（shopify-sin-shop-dev-setup / komorebi-portfolio）参照
