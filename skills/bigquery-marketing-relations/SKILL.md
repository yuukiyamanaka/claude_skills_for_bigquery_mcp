---
name: bigquery-marketing-relations
description: "マーケティング BigQuery テーブルの結合パターン定義。campaigns, ad_impressions, ad_clicks, conversions テーブル間のリレーションを提供。広告効果分析、ROAS 計算時に使用。"
---

# BigQuery マーケティングテーブル結合パターン

## 対象データセット

- **プロジェクト:** `demo_project`
- **データセット:** `demo_marketing`

| テーブル | 説明 |
|----------|------|
| campaigns | 広告キャンペーン情報 |
| ad_impressions | 広告表示ログ |
| ad_clicks | 広告クリックログ |
| conversions | コンバージョン（購入） |

## テーブル構成

```
campaigns (1) ──── (N) ad_impressions (1) ──── (0..1) ad_clicks (1) ──── (0..1) conversions
```

## 基本の結合条件

| From | To | 結合キー | 関係 |
|------|----|---------|------|
| campaigns | ad_impressions | campaign_id | 1:N |
| ad_impressions | ad_clicks | impression_id | 1:0..1 |
| ad_clicks | conversions | click_id | 1:0..1 |

## 広告効果分析の基本パターン

```sql
SELECT
  c.campaign_name,
  c.channel,
  COUNT(DISTINCT i.impression_id) as impressions,
  COUNT(DISTINCT cl.click_id) as clicks,
  COUNT(DISTINCT cv.conversion_id) as conversions,
  SUM(i.cost) as total_cost,
  SUM(cv.revenue) as total_revenue
FROM `demo_project.demo_marketing.campaigns` c
LEFT JOIN `demo_project.demo_marketing.ad_impressions` i ON c.campaign_id = i.campaign_id
LEFT JOIN `demo_project.demo_marketing.ad_clicks` cl ON i.impression_id = cl.impression_id
LEFT JOIN `demo_project.demo_marketing.conversions` cv ON cl.click_id = cv.click_id
WHERE c.status = 3  -- 終了済みキャンペーンのみ
GROUP BY 1, 2
```

## 注意事項

- 広告効果分析では LEFT JOIN を使用（クリック・CVがないケースも含める）
- ROAS 計算: `SUM(revenue) / SUM(cost)`
- conversions.order_id で EC の orders テーブルと結合可能
