# CLAUDE.md — PtBio Website

プラチナバイオ株式会社コーポレートサイト（Astro v6 + Tailwind CSS v4）。
既存Wixサイト（https://pt-bio.com）からの移行プロジェクト。

## 現状の注意点

- **文言は不正確**: 各ページのテキストはLLM生成のプレースホルダ。Wixサイトのスクリーンショットから文字起こしして正しい文言に差し替える方針
- **画像は未配置**: `public/images/` 配下は空ディレクトリのみ。Wixから手動DLして配置する予定
- **プロジェクト詳細ページはスケルトン**: `src/pages/project/*.astro` は「Wixサイトから移行時にコンテンツを差し替えてください」というプレースホルダ状態

## 技術スタック

- Astro v6（静的サイトジェネレーター）
- Tailwind CSS v4（`@tailwindcss/vite` プラグイン経由。v3以前の `tailwind.config.js` は不要）
- `@astrojs/sitemap`（sitemap.xml 自動生成）
- Node.js >= 22.12.0

## 開発コマンド

```bash
npm run dev        # localhost:4321 で開発サーバー起動
npm run build      # ./dist/ に本番ビルド
npm run preview    # ビルド結果のローカルプレビュー
```

## ナビゲーション構造

ナビゲーションは **`src/components/Header.astro`** と **`src/components/Footer.astro`** の2箇所で定義。ページ追加・削除時は両方を更新すること。

### Header（`Header.astro`）

`navItems` 配列でトップレベルのナビを定義。`children` を持つ項目はドロップダウンメニューになる。

```
ホーム → /
会社概要 → /company
Projects ▼
  ├ 赤紫蘇のデータ駆動型育種 → /project/shiso
  ├ 眼の希少疾患の遺伝子治療 → /project/drug
  ├ 卵アレルギーでも、食べられる卵を → /project/egg-for-all
  ├ 食品廃棄物を有効資源に変える → /project/agriculture
  ├ し尿処理施設をバイオDXで効率化 → /project/metagenomic-analysis-for-sewage-treatment
  └ 水産養殖の未来 → /project/aquaculture
Services ▼
  ├ ゲノム編集 → /service/genome
  └ バイオDX → /service/biodx
採用情報 → /recruitment
リソース → /resources
お知らせ → /blog
お問い合わせ → /contact
```

### Footer（`Footer.astro`）

4カラム構成。ナビリンクはハードコードされている（Header とは別管理）。

| カラム | 内容 |
|---|---|
| 会社情報 | ロゴ、社名、住所 |
| ページ | 会社概要、採用情報、リソース、お知らせ、お問い合わせ |
| サービス | ゲノム編集、バイオDX |
| お問い合わせ＋ガバナンス | フォーム、資料請求、プライバシーポリシー、不正防止、行動計画 |

## レイアウト階層

```
BaseLayout.astro        ← HTML骨格（<html>, <head>, SEO/OGP, フォント読み込み）
  └ PageLayout.astro    ← Header + <main> + Footer を組み合わせた標準レイアウト
      └ BlogLayout.astro ← ブログ記事用（日付表示、サムネ、prose スタイル）
```

すべてのページは `PageLayout` を使う。ブログ記事のみ `BlogLayout`（内部で `PageLayout` を使用）。

## ページの種類と作成パターン

### 1. 通常ページ（会社概要、採用情報など）

`PageLayout` を import して直接 HTML を書く。

```astro
---
import PageLayout from '../layouts/PageLayout.astro';
---
<PageLayout title="ページ名" description="説明">
  <section class="py-16 lg:py-24 bg-white">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <!-- コンテンツ -->
    </div>
  </section>
</PageLayout>
```

### 2. プロジェクト詳細ページ

`ProjectDetail` コンポーネントを使う。ヒーロー画像＋本文の定型レイアウト。

```astro
---
import ProjectDetail from '../../components/ProjectDetail.astro';
---
<ProjectDetail
  title="プロジェクト名"
  subtitle="English Subtitle"
  heroImage="/images/projects/xxx.jpg"
  description="OGP用の説明文"
>
  <!-- Markdown的にHTMLで本文を書く -->
</ProjectDetail>
```

### 3. サービス概要ページ（genome.astro, biodx.astro）

`ServiceCard` コンポーネントで子サービスへのリンクカードを並べるハブページ。

### 4. サービス詳細ページ（genome-1.astro, biodx-1〜3.astro, zf-nd1.astro）

`PageLayout` を使って個別にコンテンツを記述。

### 5. ブログ記事

`src/content/blog/` に Markdown を追加。`content.config.ts` で定義されたスキーマに従う。

```markdown
---
title: '記事タイトル'
date: '2026-04-01'
thumbnail: '/images/blog/image.jpg'  # 省略可
excerpt: '要約'                       # 省略可
category: 'カテゴリ名'               # 省略可
---
本文
```

- トップページの「最新情報」には日付降順で最新3件が自動表示される（`index.astro` で `getCollection('blog')` を使用）
- 個別記事のルーティングは `src/pages/blog/[...slug].astro` で動的生成

## デザインシステム

### カラー（`src/styles/global.css` で定義）

Tailwind のカスタムカラーとして使用可能（例: `bg-teal`, `text-teal-dark`, `bg-light`）。

| 名前 | 値 | 用途 |
|---|---|---|
| `teal` | `#73ACA5` | メインカラー（CTA、リンク、アクセント） |
| `teal-dark` | `#5A8F89` | ホバー状態 |
| `teal-light` | `#8FC4BE` | 薄いアクセント |
| `dark` | `#0F1B1E` | ダークセクション背景、フッター |
| `light` | `#F4F6F6` | セクション交互背景 |
| `gold` | `#D4A843` | ゴールドアクセント |
| `text` | `#333333` | 本文 |
| `text-light` | `#666666` | 補助テキスト |

### フォント

- 日本語: **Noto Sans JP**（400, 500, 700）
- 英字: **Inter**（400, 500, 600, 700）
- Google Fonts から `BaseLayout.astro` の `<head>` で読み込み

### レイアウトパターン

- セクション: `py-16 lg:py-24` + `bg-white` / `bg-light` を交互に使う
- コンテナ: `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8`
- セクション見出し: `<SectionHeading title="日本語" subtitle="English" />` を使う
- グリッド: `grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6` が基本

## デプロイ

- GitHub Actions（`.github/workflows/deploy.yml`）で `main` push 時に GitHub Pages へ自動デプロイ
- `astro.config.mjs` の `site: 'https://pt-bio.com'` がサイトURLの正規定義

## ページを追加する手順

1. `src/pages/` にファイルを作成（ファイルパス = URLパス）
2. `src/components/Header.astro` の `navItems` にナビゲーション項目を追加
3. `src/components/Footer.astro` の該当カラムにリンクを追加
4. 必要に応じて `src/pages/index.astro` のトップページにもリンクを追加
