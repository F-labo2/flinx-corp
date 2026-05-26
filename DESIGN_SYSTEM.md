# DESIGN SYSTEM — F-LINX Corporate Site

このドキュメントは、利用者さん提出のHTMLをClaude Codeで統合する際の判断基準。

---

## デザイン方針

| 項目 | 内容 |
|------|------|
| 全体トーン | 親しみやすい + 信頼感(B2Bコンサル) |
| 避けるべき | 派手な装飾、冷たい印象、過剰なアニメーション |
| 余白 | やや広め(セクション間64px、要素間16-32px) |
| 角丸 | 4〜8px(温かみを出す) |

---

## カラートークン

| トークン | 値 | 用途 |
|----------|------|------|
| `--color-main` | `#1B5E6C` (ディープティール) | 見出し・ヘッダー・フッター背景・主要ボタン |
| `--color-accent` | `#E07A3F` (温かいオレンジ) | リンク・CTA・強調・区切り |
| `--color-sub` | `#F5F1EA` (オフホワイト) | セクション背景・カード |
| `--color-bg` | `#FFFFFF` | ページ全体 |
| `--color-text` | `#2C2C2C` | 本文 |
| `--color-text-muted` | `#666666` | 補助テキスト |
| `--color-border` | `#E5E5E5` | 罫線 |

**ルール**: ハードコードカラー禁止。必ず CSS 変数経由で参照する。

---

## タイポグラフィ

| 項目 | 値 |
|------|------|
| 本文フォント | `"Noto Sans JP", "Hiragino Sans", "Meiryo", sans-serif` |
| 本文サイズ | 16px |
| 行間 | 1.8 |
| 見出し色 | `--color-main` |
| h1 (.heading-lg) | 2rem / 700 |
| h2 (.heading-md) | 1.5rem / 700 / 左にオレンジバー |
| h3 (.heading-sm) | 1.15rem / 700 |

---

## レイアウト

| クラス | 役割 |
|--------|------|
| `.container` | 最大幅1100px、中央寄せ、左右余白24px |
| `.section` | 上下padding 64px(モバイル48px) |
| `.section--sub` | サブカラー背景セクション |

---

## コンポーネント

### カード
```html
<div class="card">
  <h3 class="heading-sm">タイトル</h3>
  <p>本文</p>
</div>
```
- サブカラー背景、角丸8px、薄影
- hoverで影が濃くなる

### ボタン
```html
<a href="contact.html" class="btn-primary">お問い合わせはこちら</a>
<a href="about.html" class="btn-secondary">詳しく見る</a>
```
- `.btn-primary`: アクセントオレンジ・白文字(CTA)
- `.btn-secondary`: 透明背景・ティール枠線

### リンク
```html
<a href="..." class="link">テキスト →</a>
```

### カードグリッド(トップ事業ダイジェスト用)
```html
<div class="card-grid">
  <div class="card">...</div>
  <div class="card">...</div>
  <div class="card">...</div>
  <div class="card">...</div>
</div>
```
- デスクトップ: 4カラム
- 768px以下: 1カラム

### 情報テーブル
```html
<table class="info-table">
  <tr><th>会社名</th><td>株式会社F-LINX</td></tr>
</table>
```

### ヒーロー
```html
<section class="hero">
  <div class="container">
    <h1 class="heading-lg">...</h1>
    <p class="lead">...</p>
    <a href="..." class="btn-primary">...</a>
  </div>
</section>
```

### 区切り線(セクションタイトル下)
```html
<h1 class="heading-lg">ページタイトル</h1>
<div class="section-divider"></div>
```

---

## ヘッダー・フッター(全ページ共通・改変禁止)

`index.html` の構造をそのまま使う。各ページのみ違うのは:
- `<title>` の中身
- `<meta name="description">` の中身
- ナビゲーションの `.is-current` クラスの位置
- メインコンテンツ部分

ロゴは `onerror` 属性で画像読み込み失敗時にテキスト "F-LINX" にフォールバックする実装。
**この onerror 属性は必ず維持する**(ロゴ画像未配置時の保険)。

---

## レスポンシブ

ブレークポイントは768pxのみ。
- 768px以下では:
  - ヘッダーナビは縮小
  - 見出しサイズ縮小
  - カードグリッドは1カラム
  - フッターは1カラム
  - セクションpaddingは48px

利用者さんのHTMLにメディアクエリが入っていた場合、768px以下に統一する。

---

## 統合時のNG例 → OK例

### NG: ハードコードカラー
```css
h2 { color: #1B5E6C; }
```
### OK: CSS変数経由
```css
h2 { color: var(--color-main); }
```
または `.heading-md` クラスを使う。

### NG: 独自のクラス名で同じスタイル
```css
.my-card { background: #F5F1EA; border-radius: 8px; padding: 24px; }
```
### OK: 既存の `.card` を使う
```html
<div class="card">...</div>
```

### NG: 個別フォント指定
```css
body { font-family: 'Arial', sans-serif; }
```
### OK: CSS変数経由
```css
body { font-family: var(--font-base); }
```
(`style.css` で既に設定済みなので、利用者ファイル側で再指定不要)

---

## Claude Code への指示テンプレ

「`incoming/<file>` を `<page>.html` に統合して。DESIGN_SYSTEM.md のルールに従い、
ハードコード色や独自フォントは CSS変数・共通クラスに置き換える。
ヘッダー・フッターは index.html と同じ構造を維持。」
