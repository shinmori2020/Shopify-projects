# CLAUDE.md — sin-shop-dev (Shopify テーマ開発)

このファイルは Claude Code がこのプロジェクトで作業する際に必ず守るルールです。

## プロジェクト概要
- Shopify 開発ストア: `sin-shop-dev`（`sin-shop-dev.myshopify.com` / Dev・無料）
- テーマ: Shopify Dawn（GitHub 公式から取得した最新安定版がベース）
- 開発者: Shopify 初学者。Web制作・Next.js・VS Code + Claude Code の経験あり。
- 進め方: 1ステップずつ。コマンドは1つずつ提示し、実行結果を確認してから次に進む。

## 絶対に守るルール（安全第一）
1. **ライブ（公開中）テーマに直接 push しない。**
   - `shopify theme push` で公開テーマを上書きしない。作業は必ず開発用/下書きテーマに対して行う。
   - 公開テーマへ反映する必要があるときは、必ず事前に人間へ確認を取る。
2. **既存の schema 設定・設定値を勝手に消さない/書き換えない。**
   - `sections/*.liquid` の `{% schema %}` ブロックや `config/settings_schema.json`、
     `config/settings_data.json`、`templates/*.json` の既存項目を、説明なく削除・改変しない。
   - 変更が必要なら、何を・なぜ変えるかを先に説明し、既存項目は極力残す（追加で対応する）。
3. **theme check を必須にする。**
   - `.liquid` / テーマファイルを編集したら、コミット前に必ず `shopify theme check` を実行し、
     エラーがない状態にする（警告は内容を説明）。
4. **破壊的操作の前に確認する。**
   - ファイル削除・大量変更・`git reset --hard`・`theme pull --force` などは、実行前に必ず確認を取る。
5. **作業の節目で Git コミットする。**
   - 意味のある単位で `git add` → `git commit`。コミットメッセージは日本語で簡潔に。

## 開発フロー（基本）
- ローカルプレビュー: `shopify theme dev`（ローカル変更をリアルタイムで確認。安全）。
- 文法・規約チェック: `shopify theme check`。
- ストアへ反映（下書きとして）: `shopify theme push --unpublished`（新しい下書きテーマを作る）。
- 公開テーマの取得: `shopify theme pull`（※公開テーマを直接編集する運用は避ける）。

## コーディング方針
- Liquid / Dawn の既存の書き方・命名・構造に合わせる。独自の流儀を持ち込まない。
- 新規セクションを作るときは Dawn の既存セクションを参考にし、`{% schema %}` を適切に定義する。
- アクセシビリティ・パフォーマンス（Dawn が重視している点）を壊さない。
- 翻訳が絡む文言は `locales/` の仕組み（`{{ 'xxx' | t }}`）に従う。

## やってはいけないことの例
- 公開テーマへの無確認 push。
- `config/settings_data.json` を理由なく初期化・上書き（お店の見た目設定が消える）。
- schema の既存設定キーの削除（テーマエディタの設定が失われる）。
- シークレット（APIキー等）のコミット。

## メモ
- 本番運用に移行する際は、`config/settings_data.json` を `.gitignore` 対象にするか改めて相談する。
