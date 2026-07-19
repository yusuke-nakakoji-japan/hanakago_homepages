# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

花かご（京都府亀岡市曽我部町の和カフェ＆ドッグラン）の静的サイト。ビルドツール・フレームワーク・依存パッケージは一切なし。純粋な HTML / CSS / バニラ JS で構成される単一ページサイト。

## 開発コマンド

- **ローカルプレビュー**: `index.html` をブラウザで直接開くだけ（ビルド不要）
- **ビルド / lint / テスト**: 存在しない（該当なし）
- **デプロイ**: GitHub Pages（`main` ブランチのルートを Deploy from a branch で公開）。詳細は README.md 参照

## Git

- **アカウント**: コミットは `yusuke-nakakoji-japan` のアカウントで行う（`user.name` = `yusuke-nakakoji-japan` / `user.email` = `yusuke-nakakoji-japan@users.noreply.github.com`）。
- **自動コミット**: 一括りのタスクが完了したら、日本語のメッセージで自動的にコミットする。

## アーキテクチャ

### CSS 3層構造（重要）

スタイルは以下の3層に分かれており、この階層と役割分担を崩さないこと：

1. **`styles/tokens.css`** — デザイントークン（CSS カスタムプロパティ）のみ。色・タイポグラフィスケール・スペーシング（8px 基準）・モーション。`kit.css` から `@import` される。値のハードコードを避け、必ずここで定義された変数（`--sumi`, `--matcha-deep`, `--s-5`, `--t-lg` 等）を使う。
2. **`styles/kit.css`** — サイト共通のレイアウトプリミティブとコンポーネント（`.hk-*`）。先頭で `tokens.css` を `@import` する。
3. **`index.html` 内の `<style>` ブロック** — ページ全体のスタイルオーバーライド。すべて `.hanakago` でスコープされる。

### ページ全体のスタイルスコープ（重要）

インライン `<style>` 内の全セレクタと、ページルート `<div class="hk-page hanakago">` は `.hanakago` でスコープされている。**ページ固有の CSS を書くときは必ず `.hanakago` プレフィックスを付ける**こと（`kit.css` の共通スタイルとの分離を保つため）。`kit.css`（読み込みが先）→ インライン `<style>`（後）というソース順により、同名クラスではインライン側が優先される。

### クラス命名規則

BEM 風 + `hk-` プレフィックス：
- ブロック `hk-menu` / 要素 `hk-menu__item` / モディファイア `hk-btn--ghost`
- 状態は `is-*` クラス（`hk-menu__item.is-active`, `is-scrolled`, `is-open`, `is-in`）

### デザイントークンの命名（和の語彙）

色は和の美意識に基づく命名：
- 面（背景）: `kinari`（生成り）/ `washi`（和紙）/ `shoji`（障子・最明色）
- 墨（文字）: `sumi` → `sumi-4` の4段階
- ブランドアクセント: `hana`（花色）
- 季節アクセント: `matcha`（春夏）/ `kuchiba`（秋）/ `ai`（冬）

### JavaScript

すべてバニラ JS で `index.html` 末尾の `<script>` にインライン記述。IIFE で機能ごとに分離：ヘッダーのスクロール影、モバイルハンバーガーメニュー、画像ライトボックス、News アコーディオン、メニューカルーセル（中央スナップ検出付き）、IntersectionObserver によるスクロール表示アニメーション（`.hk-reveal` → `.is-in`）。

### フォント

`tokens.css` の Google Fonts `@import` で読み込み（Shippori Mincho = 和文見出し、Noto Sans JP = 和文本文、Cormorant Garamond = 欧文見出し、Inter = 欧文本文）。これらは代替フォントである点に注意。

## コンテンツ編集

コンテンツは日本語で `index.html` に直接記述。**News の追加**は該当セクション上部のコメント（`index.html` 内）の手順に従い、`<li class="hk-news__item">` ブロックをコピーして先頭に貼り付け、4箇所（日付・タグ・見出し・本文）を書き換える。

### レスポンシブブレークポイント

インライン `<style>` 内で定義：1080px（タブレット以下）/ 720px（モバイル）/ 420px（極小端末）。
