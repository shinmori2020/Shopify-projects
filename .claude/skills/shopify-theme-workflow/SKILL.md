---
name: shopify-theme-workflow
description: Shopify(Dawn)テーマを安全に編集するための標準ワークフロー。テーマのliquid/section/snippet/設定を変更するとき、theme devでのプレビュー、theme check、Gitコミット、下書きpushの手順を踏みたいときに使う。
---

# Shopify テーマ編集の安全ワークフロー（sin-shop-dev / Dawn）

このプロジェクトでShopifyテーマを編集するときの標準手順。

## 大原則（CLAUDE.mdと一致）
- ライブ（公開中）テーマに直接 push しない。作業は開発用/下書きテーマで行う。
- 既存の `{% schema %}` 設定・`settings_data.json`・`templates/*.json` の既存キーを勝手に消さない。値変更や追加で対応する。
- 破壊的操作（削除・大量変更・reset --hard）は事前確認。

## 変更場所の見極め（重要）
- 文言・画像・セクションの並び = **コンテンツ** → `templates/*.json` か テーマエディタ
- レイアウト・構造・部品 = **コード** → `sections/` `snippets/` `layout/` の `.liquid`
- 色・フォント・全体設定 = `config/settings_data.json`（カラースキーム等）

## 標準手順
1. プレビュー起動（ローカル・安全）:
   `cd sin-shop-dev; shopify theme dev --store sin-shop-dev.myshopify.com --store-password skeini`
2. ファイルを編集 → ブラウザ(http://127.0.0.1:9292)で即確認（ホットリロード）
3. 品質チェック: `shopify theme check`（自分の編集で新しい指摘を増やさないことを確認。Dawn既存の指摘は無理に直さない）
4. 正式保存: `git add` → `git commit`（意味のある単位・日本語メッセージ）→ 必要なら `git push`
5. ストアへ反映する場合は下書きで: `shopify theme push --unpublished`（公開テーマは上書きしない）

## トラブルメモ
- `EADDRINUSE: 9292` = 前のサーバーが残存。9292を握るnodeプロセスを停止してから再起動。
- VS Code再起動でプレビューが消えても、サーバー本体が生き残ることがある。
