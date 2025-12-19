# Claude Skills for BigQuery MCP

BigQuery MCP と組み合わせて使用する Claude Skills のサンプル集です。

## 概要

BigQuery MCP 単体では、テーブル構造は取得できてもカラム値の意味やテーブル間の結合パターンといったドメイン知識を AI に伝えることができません。本リポジトリのスキルを活用することで、ビジネス用語を正しく解釈した SQL クエリの生成が可能になります。

## スキル一覧

| スキル | 説明 |
|--------|------|
| **bigquery-mcp** | BigQuery MCP ツールの使い方ガイド |
| **bigquery-values** | EC データのカラム値定義（order_status, user_type など） |
| **bigquery-relations** | EC データのテーブル結合パターン |
| **bigquery-marketing-values** | マーケティングデータのカラム値定義（channel, status など） |
| **bigquery-marketing-relations** | マーケティングデータのテーブル結合パターン |
| **artifact-charts** | シンプルなグラフ描画用テンプレート |

## ディレクトリ構成

```
.
├── skills/
│   ├── artifact-charts/
│   ├── bigquery-mcp/
│   ├── bigquery-values/
│   ├── bigquery-relations/
│   ├── bigquery-marketing-values/
│   └── bigquery-marketing-relations/
└── sample_data/
    ├── ec/
    │   ├── orders.csv
    │   ├── order_items.csv
    │   ├── products.csv
    │   └── users.csv
    └── marketing/
        ├── campaigns.csv
        ├── ad_impressions.csv
        ├── ad_clicks.csv
        └── conversions.csv
```

## スキルのインストール

各スキルフォルダを `~/.claude/skills/` にコピーしてください。

```bash
cp -r skills/* ~/.claude/skills/
```

## サンプルデータ

検証用のサンプルデータが含まれています。BigQuery にロードして使用してください。

### EC 売上データ（demo_ec_sales）

| テーブル | 説明 |
|----------|------|
| orders | 注文情報 |
| order_items | 注文明細 |
| products | 商品マスタ |
| users | ユーザー情報 |

### マーケティングデータ（demo_marketing）

| テーブル | 説明 |
|----------|------|
| campaigns | 広告キャンペーン情報 |
| ad_impressions | 広告表示ログ |
| ad_clicks | 広告クリックログ |
| conversions | コンバージョン情報 |

## 使用例

```
プレミアム会員の先月の完了注文の売上を商品カテゴリ別に集計してください
```

スキルがロードされた状態では、以下のビジネス用語が正しく解釈されます：
- 「プレミアム会員」→ `user_type = 2`
- 「完了注文」→ `order_status = 4`

## 関連リンク

- [BigQuery MCP 公式ドキュメント](https://docs.cloud.google.com/bigquery/docs/use-bigquery-mcp)
- [Claude Skills ドキュメント](https://docs.anthropic.com/en/docs/claude-code/skills)
