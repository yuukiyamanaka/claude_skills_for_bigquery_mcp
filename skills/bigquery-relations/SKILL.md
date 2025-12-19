---
name: bigquery-relations
description: "EC サイト BigQuery テーブルの結合パターン定義。orders, order_items, products, users テーブル間のリレーションと推奨 JOIN 条件を提供。売上集計、顧客分析時に使用。"
---

# BigQuery テーブル結合パターン

## 対象データセット

- **プロジェクト:** `demo_project`
- **データセット:** `demo_ec_sales`

| テーブル | 説明 |
|----------|------|
| orders | 注文情報 |
| order_items | 注文明細 |
| products | 商品マスタ |
| users | ユーザー情報 |

## テーブル構成

```
users (1) ──── (N) orders (1) ──── (N) order_items (N) ──── (1) products
```

## 基本の結合条件

| From | To | 結合キー | 関係 |
|------|----|---------|------|
| orders | order_items | order_id | 1:N |
| order_items | products | product_id | N:1 |
| orders | users | user_id | N:1 |

## 売上分析の基本パターン

```sql
SELECT
  -- 必要なカラム
FROM `demo_project.demo_ec_sales.orders` o
INNER JOIN `demo_project.demo_ec_sales.order_items` oi ON o.order_id = oi.order_id
INNER JOIN `demo_project.demo_ec_sales.products` p ON oi.product_id = p.product_id
INNER JOIN `demo_project.demo_ec_sales.users` u ON o.user_id = u.user_id
WHERE o.order_status != 9  -- キャンセル除外
```

## 注意事項

- 売上集計時は必ず `order_status != 9` でキャンセルを除外
- 売上金額は `oi.quantity * oi.price` で計算
- order_items は必ず orders 経由で結合（直接 users と結合しない）
