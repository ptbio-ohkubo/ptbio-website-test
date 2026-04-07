# PtBio Website — プラチナバイオ株式会社 コーポレートサイト

プラチナバイオ株式会社（https://pt-bio.com）のコーポレートサイト。

## 技術スタック

| 技術 | バージョン | 用途 |
|---|---|---|
| [Astro](https://astro.build/) | v6 | 静的サイトジェネレーター |
| [Tailwind CSS](https://tailwindcss.com/) | v4 | スタイリング（`@tailwindcss/vite` 経由） |
| [@astrojs/sitemap](https://docs.astro.build/en/guides/integrations-guide/sitemap/) | v3 | sitemap.xml 自動生成 |
| Node.js | >= 22.12.0 | ランタイム |

## セットアップ

```bash
npm install
npm run dev        # 開発サーバー起動 → http://localhost:4321
npm run build      # 本番ビルド → ./dist/
npm run preview    # ビルド結果のプレビュー
```

## デプロイ

- **GitHub Actions** による自動デプロイ（`.github/workflows/deploy.yml`）
- `main` ブランチへの push で GitHub Pages にデプロイされる
- 手動トリガー（`workflow_dispatch`）にも対応

## ディレクトリ構成

```
ptbio-website/
├── src/
│   ├── pages/              # ページ（ファイルベースルーティング）
│   │   ├── index.astro         # トップページ
│   │   ├── company.astro       # 会社概要
│   │   ├── recruitment.astro   # 採用情報
│   │   ├── resources.astro     # リソース
│   │   ├── blog/               # お知らせ一覧 + 個別記事
│   │   ├── company/            # ガバナンス関連ページ
│   │   ├── contact/            # お問い合わせ + 資料請求
│   │   ├── project/            # プロジェクト詳細（6件）
│   │   └── service/            # サービス詳細
│   ├── components/         # 再利用コンポーネント
│   │   ├── Header.astro        # グローバルヘッダー（ナビゲーション定義含む）
│   │   ├── Footer.astro        # グローバルフッター
│   │   ├── Hero.astro          # トップページのヒーロー
│   │   ├── ContactForm.astro   # お問い合わせフォーム
│   │   ├── NewsCard.astro      # ニュースカード
│   │   ├── ProjectCard.astro   # プロジェクトカード
│   │   ├── ProjectDetail.astro # プロジェクト詳細テンプレート
│   │   ├── ServiceCard.astro   # サービスカード
│   │   └── SectionHeading.astro # セクション見出し
│   ├── layouts/            # レイアウト
│   │   ├── BaseLayout.astro    # HTML基本構造（SEO/OGP/フォント）
│   │   ├── PageLayout.astro    # Header + Footer を含む標準レイアウト
│   │   └── BlogLayout.astro    # ブログ記事用レイアウト
│   ├── content/
│   │   └── blog/               # ブログ記事（Markdown）
│   ├── content.config.ts   # コンテンツコレクション定義
│   └── styles/
│       └── global.css          # カスタムカラー・フォント定義
├── public/                 # 静的アセット
│   ├── images/                 # 画像（blog/, hero/, projects/, services/, team/）
│   ├── favicon.ico
│   ├── favicon.svg
│   └── robots.txt
├── .github/workflows/deploy.yml  # GitHub Pages デプロイ
├── astro.config.mjs        # Astro設定（site: https://pt-bio.com）
├── package.json
├── package-lock.json
└── tsconfig.json
```

## サイトマップ（ページ一覧）

### メインページ
| パス | 内容 |
|---|---|
| `/` | トップページ（ヒーロー、ニュース、強み、プロジェクト、サービス、会社概要、CTA） |
| `/company` | 会社概要 |
| `/recruitment` | 採用情報 |
| `/resources` | リソース |
| `/blog` | お知らせ一覧 |
| `/contact` | お問い合わせ |
| `/contact/document-request` | 資料請求 |

### プロジェクト（`/project/`）
| パス | 内容 |
|---|---|
| `/project/shiso` | 赤紫蘇のデータ駆動型育種 |
| `/project/drug` | 眼の希少疾患の遺伝子治療 |
| `/project/egg-for-all` | 卵アレルギーでも、食べられる卵を |
| `/project/agriculture` | 食品廃棄物を有効資源に変える |
| `/project/metagenomic-analysis-for-sewage-treatment` | し尿処理施設をバイオDXで効率化 |
| `/project/aquaculture` | 水産養殖の未来 |

### サービス（`/service/`）
| パス | 内容 |
|---|---|
| `/service/genome` | ゲノム編集（概要） |
| `/service/genome-1` | ゲノム編集（詳細） |
| `/service/biodx` | バイオDX（概要） |
| `/service/biodx-1` | バイオDX 詳細1 |
| `/service/biodx-2` | バイオDX 詳細2 |
| `/service/biodx-3` | バイオDX 詳細3 |
| `/service/zf-nd1` | ゼブラフィッシュND1 |

### ガバナンス（`/company/`）
| パス | 内容 |
|---|---|
| `/company/governance-policy` | ガバナンスポリシー |
| `/company/governance-plan` | ガバナンス計画 |
| `/company/governance-fraudprevention` | 不正防止 |

## デザイン仕様

### カラーパレット（`src/styles/global.css`）

| 変数名 | 値 | 用途 |
|---|---|---|
| `--color-teal` | `#73ACA5` | メインカラー（リンク、ボタン、アクセント） |
| `--color-teal-dark` | `#5A8F89` | ホバー状態 |
| `--color-teal-light` | `#8FC4BE` | 薄いアクセント |
| `--color-dark` | `#0F1B1E` | CTAセクション背景 |
| `--color-light` | `#F4F6F6` | セクション背景（交互配色） |
| `--color-gold` | `#D4A843` | ゴールドアクセント |
| `--color-text` | `#333333` | 本文テキスト |
| `--color-text-light` | `#666666` | 補助テキスト |

### フォント
- 見出し・本文: **Noto Sans JP**（Google Fonts）
- 英字: **Inter**（Google Fonts）

## ブログ記事の追加方法

1. `src/content/blog/` に Markdown ファイルを作成
2. ファイル名: `YYYY-MM-DD-slug.md`
3. frontmatter:

```markdown
---
title: '記事タイトル'
date: '2026-04-01'
thumbnail: '/images/blog/image.jpg'  # 省略可
excerpt: '記事の要約'                  # 省略可
category: 'カテゴリ名'                # 省略可
---

本文をここに記述。
```

4. トップページの「最新情報」に自動で最新3件が表示される

## 画像について

`public/images/` 配下にカテゴリごとのディレクトリがあるが、**現時点では画像ファイルは未配置**（空ディレクトリのみ）。既存の Wix サイトから手動ダウンロードして配置する予定。

| ディレクトリ | 配置する画像 |
|---|---|
| `public/images/hero/` | トップページヒーロー |
| `public/images/blog/` | ブログサムネイル |
| `public/images/projects/` | プロジェクト画像 |
| `public/images/services/` | サービス画像 |
| `public/images/team/` | チームメンバー写真 |

## 文言について

**現在の各ページのテキストはLLMによる生成であり、正確でない。** 正式な文言は、既存Wixサイトのスクリーンショットを撮影→文字起こし（OCR/手動）→各ページに再配置する方針。文言の修正が完了するまで、テキスト内容を正とみなさないこと。

## 未実装・準備中のコンテンツ

- トップページ「技術情報（Tech Note）」セクション — 「準備中です」と表示
- `public/og-image.png` — OGP画像（未配置、BaseLayout で参照あり）

## 注意事項

- `astro.config.mjs` の `site` に `https://pt-bio.com` が設定されている。ドメインを変更する場合はここを修正すること
- GitHub Actions のデプロイは Node.js 20 を使用（`package.json` の `engines` は 22.12.0 以上だが、CI では 20 を指定）— 必要に応じて統一すること
- Tailwind CSS v4 を使用。v3 以前とは設定方法が異なる（`tailwind.config.js` は不要、`@tailwindcss/vite` プラグインで動作）
