# STRETCH&STRENGTH 統合SEO監査レポート

**対象:** https://motorogu-essays-in-idleness.com/index.html
**監査日:** 2026年5月17日
**業態:** 単一拠点パーソナルジム（東京都世田谷区奥沢）/ 静的HTMLサイト（ConoHa Wing）
**監査方式:** 9専門エージェント（technical / content / schema / sitemap / performance / visual / geo / local / sxo）による並列監査

---

## 目次

1. [SEOヘルススコア](#seoヘルススコア)
2. [Critical（即時対応）](#critical即時対応--12週間)
3. [High（1ヶ月以内）](#high1ヶ月以内)
4. [Medium（3ヶ月以内）](#medium3ヶ月以内)
5. [Low（バックログ）](#lowバックログ)
6. [戦略的ポジショニング（SXO示唆）](#戦略的ポジショニングsxoからの示唆)
7. [カテゴリ別詳細](#カテゴリ別詳細)
8. [補足](#補足)

---

## SEOヘルススコア

| カテゴリ | 重み | スコア | 加重 |
|---|---|---|---|
| テクニカルSEO | 22% | 62 | 13.6 |
| コンテンツ品質 | 23% | 60 | 13.8 |
| オンページSEO（SXO） | 20% | 47 | 9.4 |
| 構造化データ | 10% | 72 | 7.2 |
| パフォーマンス（CWV） | 10% | 35 | 3.5 |
| AI検索対応（GEO） | 10% | 48 | 4.8 |
| 画像最適化 | 5% | 30 | 1.5 |
| **総合** | **100%** | | **53.8 / 100** |

ローカルSEO単独評価 = **48/100**

サイトの基礎（構造化データ、レスポンシブ対応、HTTPS）は機能しているが、**ローカル集客への変換障壁が複数存在**し、Core Web Vitalsの破綻リスクが高い状態。最大の機会損失は次の3点：

1. 電話番号がページ上に未表示（JSON-LDのみ）
2. ヒーロー動画 8.2MB が LCP 破綻要因
3. 比較サイトに勝てる土俵ではないため、GBP でのローカルパック獲得に集中すべき

---

## Critical（即時対応 — 1〜2週間）

| # | 問題 | ファイル | 対応 |
|---|---|---|---|
| 1 | `robots.txt` が存在しない | 新規作成 | サブエージェントが生成済み（リポジトリ直下に配置済み） |
| 2 | `sitemap.xml` が存在しない | 新規作成 | サブエージェントが生成済み |
| 3 | `canonical` タグが両ページ未設定 | `index.html` / `service.html` | `<head>` に追加 |
| 4 | `priceValidUntil: "2025-12-31"` が期限切れ | `service.html` | `2026-12-31` 以降に更新 |
| 5 | 電話番号がHTML本文に未表示（JSON-LDのみ） | `index.html` | `<a href="tel:+819038037040">090-3803-7040</a>` をLOCATIONとフッターに追加 |
| 6 | プライバシーポリシー・特商法表記が不在 | 新規作成 | `privacy-policy.html`、`tokusho.html` を作成しフッターからリンク |
| 7 | ヒーロー動画 8.2MB（LCP破綻） | `video/`、`index.html` | 2MB以下に再エンコード + `poster` 属性追加 |

### 即適用できるコード例

**canonical タグ:**
```html
<!-- index.html -->
<link rel="canonical" href="https://motorogu-essays-in-idleness.com/" />

<!-- service.html -->
<link rel="canonical" href="https://motorogu-essays-in-idleness.com/service.html" />
```

**電話番号（LOCATIONセクションの `<dl class="access-list">` 内）:**
```html
<dt>電話</dt>
<dd><a href="tel:+819038037040">090-3803-7040</a></dd>
```

**ヒーロー動画の改善:**
```html
<video class="hero-video"
       src="video/hero-pv.mp4"
       poster="images/hero-poster.webp"
       loop autoplay muted playsinline
       preload="metadata">
</video>
```

---

## High（1ヶ月以内）

### コンバージョン直撃の改善

- モバイルATFにCTAが存在しない → ヒーロー上に「LINEで予約」「初回相談」ボタンを配置
- トレーナー情報・差別化テキストが `index.html` に皆無 → トップに「トレーナー紹介ミニセクション」と「3つの差別化ブロック」を追加（ストレッチ1,000セッション以上、完全個室、料金67,000円〜）
- ホームページの可視テキストが約150〜200字（基準500字未満） → コンセプト+ファクトパッセージを追加（GEO引用対応も兼ねる）
- H1 が `visually-hidden` でユーザーに見えない → ヒーローにテキストオーバーレイで表示

### パフォーマンス・CWV

- 全画像を WebP へ変換（2.0MB×2件など → 計2.6MB → 約520KB）
- 全 `<img>` に `width`/`height` 属性追加（CLS防止）
- スクロール以降の画像に `loading="lazy"` 付与
- Google Fonts を非ブロッキング化（preload+onload）

### 構造化データ

- `HealthClub` に `paymentAccepted`、`currenciesAccepted`、`areaServed`、`@id`、`hasMap` を追加
- `Product` の `address` 不正プロパティを削除（service.html）
- `index.html` の `BreadcrumbList` を削除（トップページに不要）
- トレーナー資格（NSCA-CPT等保有していれば）を `Person.hasCredential` に追加

**追加例（HealthClub に areaServed）:**
```json
"areaServed": [
  {"@type": "City", "name": "世田谷区"},
  {"@type": "AdministrativeArea", "name": "奥沢"},
  {"@type": "AdministrativeArea", "name": "自由が丘"},
  {"@type": "AdministrativeArea", "name": "田園調布"}
]
```

### ローカルSEO

- GBPへの直接リンク + 「Googleで口コミを書く」ボタンを設置
- `aggregateRating` を JSON-LD に追加（GBPレビュー反映後）
- エキテン / ホットペッパービューティー / Yahoo!プレイス に掲載申請

### セキュリティ

- `.htaccess` 作成 → HSTS、X-Frame-Options、X-Content-Type-Options、Referrer-Policy を設定

```apache
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

Header set Strict-Transport-Security "max-age=31536000; includeSubDomains"
Header set X-Content-Type-Options "nosniff"
Header set X-Frame-Options "SAMEORIGIN"
Header set Referrer-Policy "strict-origin-when-cross-origin"
```

---

## Medium（3ヶ月以内）

- `llms.txt` を新規作成（GEOの即効改善）
- ローカル意図Q&Aを `index.html` に追加し、`FAQPage` スキーマも実装
- `Service` スキーマを独立ブロックとして実装（パーソナルトレーニング / ペアストレッチ）
- お客様の声セクションを追加（最低3件、可能なら `Review` スキーマ）
- ブログのテーマ転換 — 野球特化コンテンツ単独表示はジム集客とミスマッチ。「奥沢でパーソナルジムを選ぶポイント」「ストレッチ×トレーニングを組み合わせる理由」などジム集客向けに転換
- 初回体験/カウンセリングのオファーページ作成
- altテキストを「ジム内観1」→「完全個室トレーニングルーム（奥沢）」のように具体化
- フッター著作権を `&copy; 2024` → 動的化または2026に更新
- ヒーロー動画ファイル名のスペース/`&`を除去（`STRETCH&STRENGTH PV.mp4` → `hero-pv.mp4`）
- フッターSNSアイコンのタップターゲットを44×44pxに拡張

---

## Low（バックログ）

- IndexNow実装
- クリーンURL化（`.htaccess`）
- Wikidata系の権威信号
- `OpeningHoursSpecification` の `validThrough`
- トレーナー Person の `sameAs` 追加

---

## 戦略的ポジショニング（SXOからの示唆）

「奥沢 パーソナルジム」「自由が丘 パーソナルトレーニング」の SERP 1ページ目は**比較・まとめサイトが支配**しており、単一ジムの公式サイトが有機検索で勝つのは事実上困難。リソースは以下に集中すべき：

1. **ローカルパック獲得（GBP最適化）** ← 最大ROI
2. **比較サイト掲載の維持・拡大**（MELOS等への掲載先として公式サイトを整備）
3. **比較サイトが薄いロングテール**（「奥沢 ストレッチ 姿勢改善」「パーソナルジム ストレッチ特化 自由が丘」）でブログ専門性勝負

### 推奨ページ構造（SOLL）

```
[ヒーロー動画 -- 変更なし]
  ↓
[3つの差別化ポイント -- 追加]
  「ストレッチ×トレーニング一体型 / 完全個室 / 料金 67,000円〜」
  ↓
[SERVICE -- 既存に加えて施術説明を2行追加]
  ↓ → service.html への明確なリンク（「料金・初回フローを見る」）
[TRAINER ミニセクション -- 追加]
  顔写真 + 名前 + 2行経歴 + 「詳しいプロフィール」リンク
  ↓
[お客様の声 -- 追加（3件・年代・目的付き）]
  ↓
[CTA -- LINEで予約 + 「まずは見学・カウンセリング相談」の2段構え]
  ↓
[LOCATION -- 追加: 電話番号・駐輪場情報・Googleクチコミ誘導リンク]
  ↓
[BLOG -- テーマをジム集客向けに転換後に残す]
```

---

## カテゴリ別詳細

### 1. テクニカルSEO（62/100）

| 項目 | 評価 |
|---|---|
| クロール可能性 | FAIL — robots.txt / sitemap.xml が不在 |
| インデックス可能性 | 条件付きPASS — noindex は本番でコメントアウト済（OK）。canonical 未設定 |
| セキュリティ | FAIL — HSTS等のセキュリティヘッダー未設定 |
| URL構造 | PASS |
| モバイル対応 | PASS — viewport / レスポンシブ実装済 |
| Core Web Vitals | FAIL — 後述 |
| 構造化データ | PASS — 充実した実装あり |
| JS レンダリング | PASS — 静的HTMLでSSR |
| IndexNow | 未実装（低優先） |

### 2. コンテンツ品質・E-E-A-T（60/100）

- **index.html:** 35/100 — 可視テキスト約150〜200字、トレーナー情報・差別化文皆無
- **service.html:** 55/100 — トレーナー経歴・FAQ・料金など実装良好。資格名・大学名は非明示

**最重要欠損:**
- プライバシーポリシー・特商法表記の不在（YMYL隣接サービスとして重大）
- 電話番号が可視テキストに存在しない
- ホームページにトレーナー情報・コンセプトテキストなし
- `priceValidUntil` が過去日付

### 3. 構造化データ（72/100）

実装済み:
- `HealthClub`（index.html）
- `BreadcrumbList`（両ページ）
- `Product` × 2（service.html）
- `Person`（service.html）
- `FAQPage`（service.html）

**判定:**
- `HealthClub` 維持（`ExerciseGym` より適切。ストレッチ・トレーニング複合に対応）
- `FAQPage` は商業サイトのため Google リッチリザルト対象外だが、AI 引用価値で **維持推奨**
- 新規 `FAQPage` の追加は Google ベネフィット目的では推奨しない（AI 引用目的なら有効）

### 4. パフォーマンス（35/100 — 危険水域）

| 指標 | 推定値 | 判定 |
|---|---|---|
| LCP | 8〜15秒超 | Poor |
| INP | ~50〜80ms | Good |
| CLS | 0.15〜0.25 | Needs Improvement |
| FCP | 2.0〜3.5秒 | Needs Improvement |

**主因:**
- ヒーロー動画 8.2MB（poster なし、preload なし）
- 全画像 JPEG（WebP/AVIF ゼロ）、合計 2.6MB
- `<img>` に width/height・loading="lazy" なし
- 日本語フォント（Noto Serif JP）のレンダリングブロッキング

### 5. ローカルSEO（48/100）

| 次元 | 配点 | 得点 |
|---|---|---|
| GBPシグナル | 25 | 15 |
| レビュー・評判 | 20 | 2 |
| ローカルオンページ | 20 | 14 |
| NAP一貫性・引用元 | 15 | 7 |
| ローカルスキーマ | 10 | 7 |
| ローカルリンク・権威 | 10 | 3 |

**最重要課題:** レビューシグナル皆無、サイテーション構築不足、GBP直接リンク欠如。

### 6. GEO / AI検索対応（48/100）

| プラットフォーム | 推定スコア |
|---|---|
| Google AI Overviews | 45/100 |
| ChatGPT (Search) | 42/100 |
| Perplexity | 50/100 |
| Bing Copilot | 48/100 |

**対応推奨:**
- `/llms.txt` 新規作成（30分作業で全プラットフォーム改善）
- index.html に AI 直接引用可能な 134〜167 語のファクトパッセージ追加
- ローカル意図 Q&A セクション追加

### 7. SXO / ペルソナ別スコア

| ペルソナ | スコア | 主要欠損 |
|---|---|---|
| エリア外リサーチャー | 38/100 | 比較情報・差別化説明なし |
| 初回検討者・不安ユーザー | 45/100 | 体験オファー・口コミなし |
| 近隣住民ジム探し | 55/100 | 駐輪場情報・口コミ誘導なし |
| 既存顧客 | 70/100 | 電話番号非表示のみ |

---

## 補足

### 生成済みファイル

サブエージェントが既に生成済み（リポジトリ直下、main マージ時に自動デプロイ）:
- `sitemap.xml`
- `robots.txt`

### スクリーンショット

`/tmp/claude-501/seo-audit-stretchstrength/`
- `mobile_atf.png`
- `mobile_fullpage.png`
- `desktop_atf.png`
- `desktop_fullpage.png`

### 注意事項

- リポジトリ内 `vercel.json` に `X-Robots-Tag: noindex` 設定あり。Vercel にデプロイしない場合は実害なし。デプロイする場合は全インデックス無効化リスクあり
- 本番 robots.txt に PHP エラー出力が混入している報告あり（WordPress プラグイン由来）。GEO エージェント要再確認
- ローカル外部HTTPアクセスが制限されていたため、一部の項目はソースコード静的解析ベースの推定値
