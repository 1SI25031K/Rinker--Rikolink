# Rinker--Rikolink
理工系学生の活動ログを「TechLeader Score」へ定量化し、実力ベースのマッチングを実現するBashライクなAIアシスタント。

# Rinker (riko-link-rinker) 🚀

> **「エンジニアの試行錯誤を、市場価値へ同期する。」**

Rinker（リンカー）は、理工系学生の日常的な活動（GitHub, Slack, Notion）を捕捉し、企業評価に直結する独自の「TechLeader Score」へと変換・同期するBashライクなAIアシスタントです。

---

## 1. Concept: Bash-like AI Assistant
Rinkerは、エンジニアが使い慣れたターミナルの操作感をSlack上で再現します。冗長な対話ではなく、フラグ（オプション）を用いた効率的なコマンド操作を基本とします。

* **インターフェース**: Slack メンションキーワード (`@Rinker /command`)
* **設計思想**: 
    - 試行錯誤（Tinker）を価値に変える。
    - ターミナルのような等幅フォント形式によるレスポンス。
    - 複雑なプロンプトを不要にするフラグベースの指示。

---

## 2. Command Interface
スラッシュコマンドおよびメンションを通じて、以下のBashライクなコマンドを実行可能です。

### 📊 Evaluation (解析・評価)
| コマンド | オプション | 内容 | 駆動モデル |
| :--- | :--- | :--- | :--- |
| **`sum`** | `-w`, `--week` | 週間進捗要約の生成。企業提出用の下書きを作成。 | **Pro** |
| | `-d`, `--day` | 本日の日報要約の生成。 | **Sonic** |
| **`stat`** | `-v`, `--verbose` | TechLeader Scoreの指標別詳細（TES/PCS等）を表示。 | **Sonic** |
| | `-g`, `--graph` | 成長曲線のグラフ表示（Looker Studio連携）。 | **Sonic** |

### 🚀 Scouting (企業連携)
| コマンド | オプション | 内容 | 駆動モデル |
| :--- | :--- | :--- | :--- |
| **`push`** | `-f`, `--force` | 現在の要約を企業ポータルへ即時送信（Shout）。 | **Pro** |
| | `--draft` | 企業送信用の下書きを保存・確認。 | **Pro** |

### 🔔 Scheduling (RinkerBell)
| コマンド | オプション | 内容 | 駆動モデル |
| :--- | :--- | :--- | :--- |
| **`bell`** | `-ls`, `--list` | 予約可能な面談スロットを一覧表示。 | **Sonic** |
| | `-n`, `--now` | 「今すぐ面談可能」ステータスへの切り替え。 | **Sonic** |

---

## 3. Intelligence Layer (Sonic & Pro)
リクエストの緊急度と計算負荷に応じて、Geminiモデルを自動ルーティングします。

* **Rinker Sonic (Gemini 1.5 Flash)**
    * **役割**: 低レイテンシ応答、簡易QA、スコア照会、日程確認。
* **Rinker Pro (Gemini 1.5 Pro)**
    * **役割**: 高精度解析、GitHub/Slackの文脈理解、企業向け要約生成。

---

## 4. Evaluation Logic: TechLeader Score
学生の活動は、以下の数式に基づきリアルタイムおよび定時バッチで算出されます。

$$TechLeader Score = 0.4 \times TES + 0.3 \times PCS + 0.2 \times TCS + 0.1 \times TGS$$

* **TES (Technical Execution Score)**: GitHubのコミット、PR、コード複雑度。
* **PCS (Project Completion Score)**: マイルストーン達成率、納期遵守。
* **TCS (Team Contribution Score)**: Slackでのレビュー参加、技術共有、応答速度。
* **TGS (Technical Growth Score)**: 技術習得速度、対応領域の拡大。

---

## 5. Technology Stack
コスト最適化と理工系コミュニティへの適合を考慮した構成です。

* **Runtime**: Python 3.11+
* **Framework**: Slack Bolt SDK
* **AI API**: Gemini 1.5 Flash / Pro (Google AI SDK)
* **Cloud**: Oracle Cloud Infrastructure (OCI)
    * **Compute**: OCI Compute (Ampere A1)
    * **Database**: Oracle Autonomous Database (ADW/JSON)
    * **Gateway**: OCI API Gateway
* **Visual**: Google Looker Studio (埋め込み表示)

---

## 6. Architecture
Slackerのパイプライン設計 をベースに、Bashパーサーを統合しています。

```mermaid
graph TD
    User[学生/Slack] -->|@Rinker /cmd| SlackAPI[Slack Event API]
    SlackAPI -->|Webhook| Gateway[OCI API Gateway]
    Gateway -->|Trigger| Bolt[Python Bolt App]
    
    subgraph Core_Pipeline
        Bolt -->|Parse| Parser[Bash-like Parser]
        Parser -->|Route| Engine{Model Router}
        Engine -->|Low Cost| Sonic[Rinker Sonic]
        Engine -->|Deep Analysis| Pro[Rinker Pro]
    end
    
    Sonic -->|Query| DB[(Oracle DB)]
    Pro -->|Fetch Logs| GH[GitHub API]
    Pro -->|Save Result| DB
    
    DB -->|Notify| User
