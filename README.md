# mirareco  
AIを活用した「もしもの時」に備える Webアプリ「ミラレコ」

## 📘 プロジェクト共通前提
未来 + 記録（レコード） = ミラレコ  
「終活」だけでなく、災害や事故など“もしもの時/万が一”に備えるデジタル保管庫。  
本人・家族・パートナー・友人・行政・士業とも“つながり”、未来に繋ぐ記録を残せる安心体験を提供する。

---

## 🚀 公開URL
- 本番: https://mirareco.net  
- Preview: https://mirareco.vercel.app

---

## 📘 プロジェクト概要
- **アプリ名**：ミラレコ（未来＋記録）
- **対象ユーザー**：高齢者から若い大人・子育て世代・パートナー・友人・支援者（行政/士業）まで
- **目的**
  - 開発初心者が開発全体像を経験し、AI活用スキルとAIリテラシーを習得
  - プロンプト力を高め「プロンプトマスター」へ
  - 「もしもの時」の安心と「未来に届ける喜び」を両立した新しい体験を提供

---

## 📐 開発方針
- フェーズ分割（MVP → 段階的拡張）
- 技術的複雑さは少しずつ導入、常に動くプロトタイプを維持

### UI/UX
- **モバイルファースト**（片手操作OK、大きなボタン、余白多め）
- **固定ナビ**：画面下部に5大メニュー  
  - 思い出 / 保管庫 / もしもの時 / 未来に届ける / つながり
- **アクセシビリティ**：文字拡大、色弱対応、音声入力導線
- **CTA導線**：ランディングのヘッダー・ヒーロー・フッターに配置
- **関係性ラベル**：家族・パートナー（同性/異性）・友人・行政・士業

### アプリ形態
- **Webアプリ**から開始 → 将来的に**ハイブリッドアプリ（iOS/Android）**

### 🌀 画面遷移アニメーション指針（全体統一）
- 目的：切替の手がかりを与え、迷いを減らす（高齢者配慮）
- ライブラリ：Framer Motion（App Router対応）
- 基本動作：
  - **フェード & スライド（Y+8px）**：0.24s / ease-out
  - **入れ替わり**：旧画面は 0.16s でフェードアウト → 新画面が 0.24s でフェードイン
- 一貫ルール：
  - 遷移時間：0.2–0.4秒に統一（デフォルト 0.24s）
  - 遷移方向：原則 **下から上（+8px → 0）** で出現
  - スクロール位置：遷移後は先頭へ（アクセシビリティ重視）
  - **低刺激配慮**：`prefers-reduced-motion` を尊重（アニメ無効）
- 適用範囲：`/app/**` のページ全体（タブ切替含む）
- 例外：**緊急系UI**は別途ガイド（点滅/二段階確認、強調表現）
- パフォーマンス：FPS低下時はフェードのみ（フォールバック）

#### ✅ 受け入れ基準（アニメーション）
- スマホで 1 フレーム当たり視認性良好（カクつき < 1回/遷移）
- 連打しても破綻しない（二重マウント/し残りなし）
- `prefers-reduced-motion: reduce` で全アニメ無効
- Lighthouse パフォーマンス 90+ を維持（同一端末条件）

---

## 🛠 技術スタック
- フロントエンド：Next.js（App Router）
- バックエンド：FastAPI（予定）
- DB：Supabase（PostgreSQL / RLS）
- インフラ：Vercel（将来的にAWS移行も視野）
- ドメイン：mirareco.net

---

## 🌐 ランディングページ構成
- `/` = 公開ランディング  
  - `components/marketing/` 配下に Header, Hero, Features, Steps, Trust, CTA, Footer を分離  
  - SEO/OGP用の `metadata` 設定  
  - モバイルファースト設計 + ariaラベルでアクセシビリティ対応  
- `/app` = アプリ本体（タブUI）

---

## 🎨 ランディングページ仕様（MVP決定版）

### 情報設計（セクション順）
1. Hero（大見出し・説明・主/従CTA）
2. Features（3カード：完結性／もしもの時／未来メッセ）
3. How to Start（4ステップ）
4. Security / Trust（3項目：RLS／暗号化／緊急時ガード）
5. Pricing（無料／家族・パートナーパス／ストレージ）
6. FAQ（3問）
7. CTA（再掲）

> 参考：セクションの並びと視線誘導は「みんなの銀行」LPのリズムを参考（構成のみ、内容は独自）。  
> https://www.minna-no-ginko.com/

### タイポグラフィ & 余白（Tailwind目安）
- H1: `text-4xl sm:text-5xl md:text-6xl font-semibold leading-tight tracking-tight`
- H2: `text-2xl sm:text-3xl font-semibold tracking-tight`
- 本文: `text-base sm:text-lg text-neutral-600`
- 小見出し/注釈: `text-sm text-neutral-500`
- カード: `rounded-2xl border border-neutral-200 p-6`
- セクション間: `py-14 sm:py-16`／セクション区切り: `border-t border-neutral-200`

### アクセシビリティ
- 文字サイズとコントラストの初期設定は Onboarding で選択
- ボタンは最低44pxのタップ領域／フォーカスリング有効
- 緊急機能は**二段階確認**（誤タップ防止）

### 実装ファイル（Next.js App Router）
- `app/page.tsx` … LP本体（Canvas納品コードを使用）
- `app/layout.tsx` … `metadata`（SEO/OGP）
- `public/` … `hero-app.png`, `feat-*.png`, `logo.svg`, `ogp.png`

### 受け入れ基準（LP）
- スマホ幅でヒーローが1スクロール内に収まる
- CLS（初回読み込み）で見出し・ボタンがズレない（画像は`width/height`指定）
- セクション見出しがスクロールで**一定ピッチ**に現れ、CTAまで迷いなく到達
- Lighthouse モバイルで Performance/Accessibility/Best Practices/SEO いずれも 90+ を目標

### 注意（リスペクト・リーガル）
- 構成・余白・タイポの“バランス”は参考にするが、文言・画像・図版は**オリジナル**を使用
- 商標・ロゴ・固有ビジュアルの模倣は禁止

---

## 🤖 AI（ChatGPT）の役割
- 設計サポート  
- 実装支援（コード例＋初心者向けメモ解説）  
- 学習補助（専門用語解説）  
- 資料作成（PDF/Markdown）  
- 進捗管理（日誌/タスク整理）  
- プロンプト学習（練習/改善提案）

---

## 💬 チャット構成と役割メモ
- 設計チャット：要件定義・設計の相談
- 実装チャット：フロント・バックエンド・DBの技術サポート、コード解説
- 学習チャット：専門用語や工程を初心者向けに解説
- 資料チャット：要件定義書・設計書・PDFなどの資料作成
- 進捗管理チャット：日誌形式で進捗を記録し、タスクを整理
- プロンプト学習チャット：プロンプトの練習・改善アドバイス

---

##📑 活用の仕方（クリックで展開）

- 新しいチャットを作るとき：冒頭にこの概要を貼ると文脈が途切れない  
- 設計や実装で迷ったとき：開発方針や技術スタックを確認  
- ドキュメント作成時：README.md や Notion に貼り付け、最新版を維持  
- 進捗管理時：開発方針に照らしてタスクの優先度を確認  
- 学習チャットやプロンプト練習時：AIの役割を確認して活用      

---

## 🧭 コア体験
1. もしもの時：緊急連絡先 / 災害メッセ / 身分証保管 / 緊急アクセスカード  
2. 保管庫：保険・医療・財産などの書類保存  
3. 未来に届ける：未来メッセージ / タイムカプセル（AIサポート対応）  
4. 思い出：写真・動画・音声アルバム  
5. つながり：家族・パートナー・友人・行政・士業を招待 / 共同編集 / 見守り通知  

---

## 💰 マネタイズ
- **無料**：思い出1件、緊急連絡先1件、未来メッセ1通、招待1人  
- **家族・パートナーパス**：招待無制限、見守り通知、共同編集、関係性ラベル拡張  
- **ストレージ課金**：50GB/200GB追加  
- **プレミアム**：AI文章サポート / AI要約 / 演出強化  
- **外部連携**：士業・保険・葬儀社・行政と提携

---

## 🔗 プロジェクトファイル構成（docs/）
- README.md  
- mvp-memo-spec.md  
- ui-wireframes.md  
- data-model.md  
- api-design.md  
- pricing-and-partners.md  
- progress-log.md  
- todo-backlog.md  

---

## 📂 リポジトリ構成（どこに何を置くか）
```
mirareco/
├─ docs/                               # 仕様・設計・議事録など
│  └─ mvp-memo-spec.md
├─ frontend/                           # Next.js (アプリ本体)
│  ├─ app/                             # App Router（ページ/レイアウト）
│  │  ├─ layout.tsx                    # 全体レイアウト（メタ/遷移アニメ適用）
│  │  ├─ page.tsx                      # トップ = ランディングページ
│  │  ├─ login/
│  │  │  └─ page.tsx                   # ログイン（後でSupabase Auth接続）
│  │  └─ signup/
│  │     └─ page.tsx                   # 新規登録（後でSupabase Auth接続）
│  ├─ components/                      # 再利用UI
│  │  └─ ClientTransition.tsx          # Framer Motionでページ遷移アニメ
│  ├─ lib/                             # APIクライアント/ユーティリティ
│  │  └─ supabaseClient.ts             # Supabase接続（後で追加）
│  ├─ public/                          # 画像や静的ファイル
│  ├─ styles/ (または app/globals.css) # グローバルCSS/Tailwind
│  ├─ package.json
│  └─ next.config.ts
├─ README.md                           # このファイル
└─ (将来) backend/                     # FastAPI（Python）を追加予定
```

---

## 🛠 セットアップ手順（起動方法）

### 1) 取得〜起動（ローカル）
```bash
# プロジェクト取得
git clone https://github.com/linkist0622/mirareco.git
cd mirareco/frontend

# 依存インストール
npm install

# 開発サーバー起動（http://localhost:3000）
npm run dev
```

### 2) 必要ライブラリ（ランディングページ用アニメーション）
```bash
# Framer Motion（ページ遷移アニメ）
npm i framer-motion
```

### 3) 環境変数（Supabaseを使う場合）
- ローカル：`frontend/.env.local` を作成
```bash
NEXT_PUBLIC_SUPABASE_URL=（SupabaseのProject URL）
NEXT_PUBLIC_SUPABASE_ANON_KEY=（anon key）
```
- Vercel（本番）：Project → Settings → Environment Variables に同名で追加  
  追加後、**Redeploy** で反映。

### 4) よく使うスクリプト（package.json）
```bash
npm run dev        # 開発サーバー
npm run build      # 本番ビルド
npm start          # ビルド済みを起動（ローカル確認）
npm run lint       # コードチェック
```

### 5) デプロイ（Vercel）
- `main` ブランチへ **git push** すると、Vercelが自動ビルド＆本番反映。  
- 公開URL：`https://mirareco.net`

---

## 🔒 セキュリティ
- Supabase RLS（行レベル制御）
- 保存/通信時の暗号化
- 緊急アクセス権限の最小化

---

## ✅ MVP基準
- 5大メニューが動作
- 無料枠機能が制御される
- 家族パスで招待無制限・通知・共同編集が有効
- RLSが有効で本人以外はデータ参照不可

---

## 🛠 開発フロー（Step by Step）

> 目的：常に“動くプロトタイプ”を維持しながら、MVP → 拡張へ段階的に進める

### Step 1. ランディングページ構築（/）
- やること：
  - `app/page.tsx` を作成し、公開LPを実装
  - `components/marketing/`（Header / Hero / Features / Steps / Trust / CTA / Footer）へ分離
  - `metadata` で SEO/OGP、`aria-*` でアクセシビリティ対応
- 完了条件：
  - スマホでの読みやすさ・CTA導線を確認（ヘッダー/ヒーロー/フッターの3点）
  - Vercelにデプロイし、表示確認ができる

### Step 2. アプリ本体のスケルトン（/app）
- やること：
  - 5大メニュー（思い出 / 保管庫 / もしもの時 / 未来に届ける / つながり）を `/app` 配下に用意
  - 下部固定ナビ（タブUI）で遷移可能に
- 完了条件：
  - 各メニューへ遷移・戻るが可能（中身は空でもOK）
  - モバイル片手操作で違和感がない

### Step 3. Supabase接続 & DB設計
- やること：
  - Supabase プロジェクト作成、`NEXT_PUBLIC_SUPABASE_URL` / `ANON_KEY` 設定
  - テーブル：`profiles`, `memories`, `locker_items`, `future_msgs`, `links`
  - **RLS**（Row Level Security）ポリシー設定（本人のみ閲覧/編集、共有権限は最小限）
- 完了条件：
  - ログインユーザーで自分のレコードだけ読める／他人のは読めない
  - 1件のレコードCRUDが通る

### Step 4. MVP要件の実装
- やること（無料枠）：
  - 思い出1件、緊急連絡先1件、未来メッセ1通、招待1人の上限制御
- やること（家族・パートナーパス）：
  - 招待無制限、見守り通知（未ログイン○日で通知）、共同編集
- 完了条件：
  - 受け入れ基準を `docs/mvp-memo-spec.md` と **progress-log** にチェック
  - RLSと権限テンプレ（閲覧/編集/緊急アクセス）が基本動作

### Step 5. 課金モデル実装
- やること：
  - 家族・パートナーパス（サブスク）、ストレージ追加（容量課金）
  - プレミアム：AI文章支援/要約/演出（トグルで段階導入）
  - 料金・制約を `docs/pricing-and-partners.md` に明文化
- 完了条件：
  - UI上にロック表示とアップグレード導線、課金状態による機能切り替え

### Step 6. 外部連携（拡張フェーズ）
- やること：
  - 士業・保険・葬儀社・自治体との連携ポイントを設計
  - 相談導線（もしもの時→専門家相談）/ 行政・士業アカウントの追加
- 完了条件：
  - 1連携のPoC（モックでも可）と、改善点メモ

### Step 7. ハイブリッド化（iOS/Android）
- やること：
  - Next.js +（Capacitor / Expo）でラップし、通知や共有シート等を検証
- 完了条件：
  - デバイス実機でLP〜/app 遷移が快適、主要フローが動く

---

### 運用ルール（短文メモ）
- **常に動く**：大きな変更は小さく分割して早くマージ
- **1ファイル1責務**：LP用UIは `components/marketing/` に集約
- **権限は最小**：RLSと共有権限は“デフォルト最小、必要に応じて開く”
- **記録する**：マイルストーンごとに `progress-log.md` を更新

---

## 🗺️ ロードマップ
- [x] Vercelデプロイ & 独自ドメイン設定  
- [x] README整備  
- [ ] MVP実装（Next.jsルーティング、Supabase接続、RLS、無料枠制御、家族パス）  
- [ ] FastAPI接続  
- [ ] 認証（Supabase Auth）  
- [ ] ハイブリッド化（iOS/Android）

---

## 📚 初心者メモ
- **RLS**：Row Level Security（行単位のアクセス制御）  
- **JWT**：ログイントークンの仕組み  
- **OGP**：SNS共有時に出るサムネイル情報  
- **CTA**：Call To Action（行動を促すボタンや導線）
