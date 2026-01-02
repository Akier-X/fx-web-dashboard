# 🌐 FX Web Dashboard

**リアルタイムFX取引監視ダッシュボード**

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue)](https://www.python.org/)
[![Dashboard Validation](https://github.com/Akier-X/fx-web-dashboard/actions/workflows/dashboard-validation.yml/badge.svg)](https://github.com/Akier-X/fx-web-dashboard/actions/workflows/dashboard-validation.yml)
[![Flask](https://img.shields.io/badge/Flask-3.0%2B-green)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📋 目次

- [概要](#-概要)
- [機能](#-機能)
- [スクリーンショット](#-スクリーンショット)
- [クイックスタート](#-クイックスタート)
- [使用方法](#-使用方法)
- [API仕様](#-api仕様)
- [技術スタック](#-技術スタック)
- [カスタマイズ](#-カスタマイズ)

---

## 🎯 概要

FX取引システムの**リアルタイム監視ダッシュボード**です。

Flask + HTML/CSS/JavaScriptで構築され、取引状況・損益・予測・チャートを30秒ごとに自動更新して表示します。

### 主な特徴

- ✅ **リアルタイム監視** - 30秒ごとに自動更新
- ✅ **現在価格・予測表示** - USD/JPYの現在価格と次の予測方向
- ✅ **損益グラフ** - リアルタイムの収益推移を可視化
- ✅ **取引統計** - 勝率、総損益、シャープレシオ
- ✅ **適応学習状況** - モデル更新タイミングと精度
- ✅ **価格チャート** - 過去6ヶ月のUSD/JPY推移（移動平均・ボリンジャーバンド）
- ✅ **モバイル対応** - レスポンシブデザイン

---

## 🎨 機能

### 1. リアルタイムステータス

```
┌─────────────────────────────────┐
│  システムステータス              │
│  ● 稼働中                       │
│  取引可能                       │
└─────────────────────────────────┘
```

- システムの稼働状態
- 取引可能/不可の表示
- 最終更新時刻

### 2. 現在価格・予測

```
┌─────────────────────────────────┐
│  USD/JPY                        │
│  156.95円                       │
│                                 │
│  次の予測: ↑ 上昇 (92.5%)      │
└─────────────────────────────────┘
```

- リアルタイム価格
- 次の取引方向予測
- 予測確率（信頼度）

### 3. 損益グラフ

```
収益推移 (JPY)
│
│     ╱╲
│    ╱  ╲    ╱
│   ╱    ╲  ╱
│  ╱      ╲╱
└────────────────→ 時間
```

- 時系列での損益推移
- 累積収益の可視化

### 4. 取引統計

```
┌─────────────────────────────────┐
│  総損益:    +32,050円 (+32%)   │
│  勝率:      65.0%               │
│  Sharpe比:  10.29               │
│  最大DD:    -2,100円 (-2.1%)   │
│  取引回数:  120回               │
└─────────────────────────────────┘
```

### 5. 適応学習状況

```
┌─────────────────────────────────┐
│  オンラインモデル訓練済み        │
│  更新バッファ: 45/50取引        │
│  次回更新まで: 5取引            │
│  現在の適応精度: 94.2%          │
└─────────────────────────────────┘
```

- モデル訓練状態
- 更新タイミング
- 適応モデルの精度

### 6. 価格チャート

```
USD/JPY 価格推移（過去6ヶ月）

価格(円)
│  BolingerBand上限
│     移動平均(MA25)
│  BolingerBand下限
│      実際の価格
└─────────────────→ 日付
```

- 6ヶ月分の価格推移
- 移動平均線（7日、25日、50日）
- ボリンジャーバンド
- 日次リターン
- ボラティリティ

---

## 🚀 クイックスタート

### 1. インストール

```bash
git clone https://github.com/Akier-X/fx-web-dashboard.git
cd fx-web-dashboard
pip install -r requirements.txt
```

### 2. ダッシュボード起動

```bash
# Pythonで直接起動
python web_dashboard.py

# またはバッチファイル（Windows）
start_dashboard.bat
```

### 3. ブラウザでアクセス

```
http://localhost:5000
```

自動的にブラウザが開きます（`start_dashboard.bat`使用時）

---

## 📖 使用方法

### 基本的な起動

```bash
# ダッシュボードのみ起動
python web_dashboard.py
```

**注意**: ダッシュボードは取引ボット（`adaptive_learning_bot.py`等）と一緒に使用することを想定しています。

### 取引ボットと一緒に使用

```bash
# ターミナル1: 取引ボット起動
cd ../fx-adaptive-trading-system
python start_adaptive_demo.py

# ターミナル2: ダッシュボード起動
cd ../fx-web-dashboard
python web_dashboard.py
```

### 価格チャート手動更新

ダッシュボード上の「🔄 チャート更新」ボタンをクリック

または:

```bash
python show_price_chart.py
```

---

## 🔌 API仕様

### GET /api/status

取引システムの現在状態を取得

**レスポンス**:
```json
{
  "current_price": 156.95,
  "prediction": "UP",
  "confidence": 0.925,
  "total_profit": 32050,
  "win_rate": 0.65,
  "sharpe_ratio": 10.29,
  "max_drawdown": -2100,
  "trade_count": 120,
  "is_running": true,
  "will_trade": true,
  "online_model_trained": true,
  "update_buffer_size": 45,
  "timestamp": "2026-01-03 12:30:45"
}
```

### GET /api/chart

価格チャートを生成

**レスポンス**:
```json
{
  "status": "success",
  "chart_url": "/chart_image"
}
```

### GET /chart_image

生成されたチャート画像を取得

**レスポンス**: PNG画像

---

## 📁 ディレクトリ構造

```
fx-web-dashboard/
├── README.md                    # このファイル
├── requirements.txt             # Python依存関係
├── .gitignore                   # Git除外設定
│
├── web_dashboard.py             # Flaskアプリケーション（メイン）
├── show_price_chart.py          # 価格チャート生成スクリプト
├── start_dashboard.bat          # Windows用起動スクリプト
├── open_dashboard.html          # ブラウザ自動オープン用HTML
│
├── templates/                   # HTMLテンプレート
│   └── dashboard.html           # ダッシュボードUI
│
├── static/                      # 静的ファイル（.gitignore）
│   ├── css/                    # CSSファイル
│   └── js/                     # JavaScriptファイル
│
└── outputs/                     # 生成ファイル（.gitignore）
    └── usd_jpy_price_chart.png  # 価格チャート画像
```

---

## 🛠️ 技術スタック

### バックエンド

- **Flask 3.0+** - Webフレームワーク
- **matplotlib** - チャート生成
- **yfinance** - Yahoo Financeデータ取得
- **pandas, numpy** - データ処理

### フロントエンド

- **HTML5** - マークアップ
- **CSS3** - スタイリング（グラデーション、カード、レスポンシブ）
- **JavaScript (Vanilla)** - インタラクティブ機能

### 自動更新メカニズム

```javascript
// 30秒ごとに自動更新
setInterval(updateDashboard, 30000);

// 5分ごとにチャート更新
setInterval(refreshChart, 300000);
```

---

## 🎨 カスタマイズ

### 更新間隔の変更

`templates/dashboard.html`を編集:

```javascript
// 30秒 → 10秒に変更
setInterval(updateDashboard, 10000);  // 10秒

// 5分 → 3分に変更
setInterval(refreshChart, 180000);    // 3分
```

### ポートの変更

`web_dashboard.py`を編集:

```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=False)  # 5000 → 8080
```

### チャートの期間変更

`show_price_chart.py`を編集:

```python
# 6ヶ月 → 1年に変更
start_date = end_date - timedelta(days=365)  # 180 → 365
```

### デザインの変更

`templates/dashboard.html`のCSSセクションを編集:

```css
/* カラーテーマ変更 */
body {
    background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
    /* 青系 → 好みの色に変更 */
}
```

---

## 📊 ダッシュボードの見方

### ステータスインジケーター

- **● 稼働中** (緑) - システム正常稼働
- **● 停止中** (赤) - システム停止
- **取引可能** - 現在取引中
- **取引停止** - 取引なし（待機中）

### 予測表示

- **↑ 上昇 (XX%)** - 上昇予測（確率XX%）
- **↓ 下降 (XX%)** - 下降予測（確率XX%）
- **― 見送り** - 信頼度不足のため取引見送り

### 損益の色分け

- **緑** - プラス収益
- **赤** - マイナス損失
- **グレー** - ±0円

---

## 🔒 セキュリティ

- 本番環境では`debug=False`を設定
- 外部アクセスを許可する場合は認証を追加推奨
- APIエンドポイントにレート制限を設定推奨

---

## 🔗 関連リポジトリ

- [fx-adaptive-trading-system](https://github.com/Akier-X/fx-adaptive-trading-system) - 本番取引システム（このダッシュボードを含む統合版）
- [fx-model-research](https://github.com/Akier-X/fx-model-research) - モデル研究
- [fx-data-pipeline](https://github.com/Akier-X/fx-data-pipeline) - データ収集

---

## 📝 ライセンス

MIT License

---

## 👤 作成者

**Akier-X**

- GitHub: [@Akier-X](https://github.com/Akier-X)
- Email: info.akierx@gmail.com

---

**💡 ヒント**: このダッシュボードは[fx-adaptive-trading-system](https://github.com/Akier-X/fx-adaptive-trading-system)に統合されています。スタンドアロン版として独立して使用することも可能です。
