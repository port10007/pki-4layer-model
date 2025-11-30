# 🌐 PKI 4階層統合モデル（mnr.dev Edition）

本ドキュメントは、Public Key Infrastructure（PKI）を  
**暗号目的・TLS検証構造・CAガバナンス・証明書ライフサイクル**  
という4つの軸から統合的に整理したモデルです。

PKI Zoo の抽象化研究に基づき、実務者・学習者・研究者に向けて  
「PKI 全体像を一枚の構造図で理解できる」ことを目的としています。

---

# 🟦 背景と必要性（なぜ4階層モデルが必要なのか）

PKI は20年以上存在するにもかかわらず、その概念構造は  
複数の分野に断片化されています。

- RFC 5280 / 6960 / 8446：暗号・証明書の技術仕様  
- CABF：ガバナンス・運用基準  
- 各CA：独自のライフサイクル運用  
- ブラウザ：独自の失効戦略や信頼ストア

この“分断”により、PKI を総合的に理解することが困難で、  
以下のような問題が恒常的に発生しています：

- 原因が別レイヤにあるために「想定外」に見える障害  
- 実装者が仕様意図を誤解することで起きるバグ  
- CA・ブラウザ・依存アプリ間の認識のズレ  
- テストフレームワークが全体構造を捉えきれない  
- 研究が部分的になり、全体系を扱えない

**PKI は本来“4つの次元”で成り立っていたが、  
誰もそれを1つのモデルとして説明してこなかった。**

---

# 🟦 今このモデルが必要とされる理由

## 1. PKI依存プロトコルの複雑化
TLS1.3、QUIC、ECH、PQC、CT、CRLite等により  
「実装」と「ガバナンス」が密接に結びついている。

## 2. 大規模障害の多くが“レイヤ間のギャップ”で発生
例：

- AIA/CDP の誤設定（Lifecycle × Implementation）  
- OCSP soft-fail vs hard-fail（Governance × Implementation）  
- クロスサインの経路分岐（Purpose × Lifecycle）  
- CDN設定ミスによる revocation failure（Implementation × Lifecycle）

## 3. 学術と産業で共通言語が存在しない
- 研究者：形式手法・暗号  
- 産業：運用・プロセス  
両者を統合する枠組みがなかった。

## 4. テスト・検証は構造化されたモデルを必要とする
Curl Zoo、PKI Zoo、QUIC Zoo が示す通り、  
「単一レイヤ理解」では不十分。

---

# 🟦 本モデルの貢献（Contribution）

4階層モデルは次を提供します：

- PKI を統合的に捉えるための“単一の語彙”  
- あらゆるPKI問題を分類できる構造マップ  
- PKI Zoo のテスト生成・シナリオ設計の基盤  
- RFCと現場運用を橋渡しする実践的抽象化  
- 新規学習者から研究者まで共通で使える理論枠組み  

---

# ◆ 第1層：暗号基盤（PKIの目的）

PKI が提供する3つの基本的セキュリティ特性：

1. **Authentication（認証）**  
   公開鍵が正当な主体に属することを保証する。

2. **Integrity（完全性）**  
   データや通信が改ざんされていないことを保証する。

3. **Confidentiality（機密性）**  
   通信内容を第三者から秘匿する。

🔍 *暗号理論の標準教科書（Boneh/Shoup, Katz/Lindell）と整合。*

---

# ◆ 第2層：TLSの検証構造（実装 / RFC5280）

TLS クライアントが行う3つの主要検証。  
PKI Zoo の **Name / Path / Revocation** の元となる構造。

1. **Name（名前の正当性）**  
   Subject/SAN が接続先ホスト名と一致する。

2. **Path（経路の正当性）**  
   Leaf → Intermediate → Root のチェーンが正しい。

3. **Revocation（失効による有効性）**  
   CRL / OCSP により失効していないこと。

🔍 *OpenSSL / BoringSSL / NSS / Windows CAPI の挙動と一致。*

---

# ◆ 第3層：CAガバナンス（組織層）

実運用で PKI を安全に維持するための管理原則。

1. **Identity（実世界での身元確認 / RA / KYC）**  
2. **Authorization（用途制限・役割管理 / EKU / Policy）**  
3. **Accountability（監査・非否認性・ログ管理）**

---

# ◆ 第4層：証明書ライフサイクル（現場作業）

CA/RA が毎日行う作業領域：

1. **Registration** – CSR受付、属性登録、本人確認  
2. **Lifecycle** – 発行、更新、ロールオーバ、失効管理  
3. **Validation** – OCSP対応、CRL発行、問い合わせ対応、監査準備  

---

# 🧩 The Four Layers in One Diagram（テキスト図）

```
+--------------------------------------------------------------+
| Layer 1: Cryptographic Foundation (Purpose)                  |
| Authentication / Integrity / Confidentiality                 |
+--------------------------------------------------------------+

+--------------------------------------------------------------+
| Layer 2: TLS Validation Structure (RFC 5280)                 |
| Name / Path / Revocation                                     |
+--------------------------------------------------------------+

+--------------------------------------------------------------+
| Layer 3: CA Governance (Organizational Principles)           |
| Identity / Authorization / Accountability                    |
+--------------------------------------------------------------+

+--------------------------------------------------------------+
| Layer 4: PKI Lifecycle Operations (Operational Reality)      |
| Registration / Issuance / Renewal / Revocation / Validation  |
+--------------------------------------------------------------+
```

---

# 📌 このモデルの使い方（mnr.dev 用）

PKI を正しく理解するには、  
**暗号・TLSの挙動・ガバナンス・実運用** を同時に見る必要があります。

本モデルは以下の用途に有効です：

- PKI を初めて学ぶ人  
- TLS証明書検証を分析するエンジニア  
- 企業内CAの運用担当者  
- 電子政府PKIの技術者  
- PKI研究者・テスト設計者  

学習・テスト・研究のための **“統合的地図”** として使えます。

---

# 🟦 結論（Conclusion）

PKI 4階層統合モデルは、  
従来バラバラに扱われていた **技術・実装・ガバナンス・ライフサイクル** を  
一つの構造として整理することで：

- 学習コストを劇的に下げ  
- 実装バグを減らし  
- CA とブラウザの期待値を揃え  
- テスト設計を体系化し  
- 次世代PKI（PQC / ECH / CRLite / Delegated Credentials 等）の理解基盤を提供する  

“現代PKIを理解するための実用モデル” です。

---

# 📄 推奨ライセンス：CC BY 4.0

本資料の共有・再利用に最適なライセンスです：

- 再配布・改変自由  
- 商用利用可  
- ただし著作者（Minoru Tachibana）の表示が必要  

全文：https://creativecommons.org/licenses/by/4.0/

