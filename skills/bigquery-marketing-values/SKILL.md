---
name: bigquery-marketing-values
description: "マーケティング BigQuery テーブルのカラム値定義。channel や status などの数値コードとビジネス用語の対応を提供。広告効果分析、キャンペーン分析、ROAS 計算時に使用。"
---

# BigQuery マーケティングカラム値定義

## campaigns テーブル

### channel
| 値 | 意味 |
|----|------|
| google_ads | Google 広告 |
| facebook | Facebook 広告 |
| instagram | Instagram 広告 |
| email | メールマーケティング |

### status
| 値 | 意味 |
|----|------|
| 1 | 予定（未開始） |
| 2 | 実行中 |
| 3 | 終了 |

**よく使う条件:**
- 実行中キャンペーン: `status = 2`
- 終了済み: `status = 3`

## ad_impressions テーブル

### device_type
| 値 | 意味 |
|----|------|
| mobile | スマートフォン |
| desktop | PC |
| tablet | タブレット |

## 主要指標の計算

- **CTR（クリック率）**: clicks / impressions
- **CVR（コンバージョン率）**: conversions / clicks
- **CPA（獲得単価）**: total_cost / conversions
- **ROAS**: revenue / total_cost
