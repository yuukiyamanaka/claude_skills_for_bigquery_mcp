---
name: bigquery-mcp
description: "BigQuery MCP ツールのエキスパート。データ探索、スキーマ確認、SQL クエリ実行に使用。BigQuery のデータセット、テーブル、分析クエリの実行時にトリガー。"
---

# BigQuery MCP ツールガイド

## 概要

BigQuery MCP は以下の 5 つのツールを提供：
- **list_dataset_ids** - プロジェクト内のデータセット一覧
- **get_dataset_info** - データセットのメタデータ取得
- **list_table_ids** - データセット内のテーブル一覧
- **get_table_info** - テーブルスキーマとメタデータ取得
- **execute_sql** - SQL クエリ実行

## ツール選択ガイド

| 目的 | ツール | 使用場面 |
|------|--------|----------|
| データ探索 | `list_dataset_ids` | 最初のステップ：どのデータセットがあるか確認 |
| データセット詳細 | `get_dataset_info` | 説明、ラベル、ロケーション取得 |
| テーブル探索 | `list_table_ids` | 特定データセット内のテーブル一覧 |
| スキーマ確認 | `get_table_info` | カラム名、型、説明を取得 |
| データ取得 | `execute_sql` | SELECT、集計、結合クエリ実行 |

## 推奨ワークフロー

```
1. list_dataset_ids
   └─> 対象データセットを特定

2. list_table_ids(dataset_id)
   └─> 関連テーブルを探索

3. get_table_info(dataset_id, table_id)
   └─> クエリ前にスキーマ確認

4. execute_sql(query)
   └─> 最適化したクエリを実行
```

## ツールリファレンス

### list_dataset_ids

プロジェクト内の全データセット ID を取得。

**戻り値:** データセット ID の配列

### get_dataset_info

**パラメータ:** `dataset_id`（必須）

**戻り値:** 説明、ラベル、ロケーション、作成日時

### list_table_ids

**パラメータ:** `dataset_id`（必須）

**戻り値:** テーブル ID の配列

### get_table_info

**パラメータ:** `dataset_id`（必須）、`table_id`（必須）

**戻り値:** カラム名・型・説明、パーティション設定、クラスタリング、行数

### execute_sql

**パラメータ:** `query`（必須）

**戻り値:** クエリ結果

## ベストプラクティス

### スキーマを先に確認

```
# NG: カラム名を推測
execute_sql("SELECT user_id FROM events")

# OK: スキーマ確認後にクエリ
1. get_table_info で実際のカラム名を確認
2. 正確なカラム名で execute_sql
```

### 完全修飾テーブル名を使用

```sql
SELECT * FROM `project_id.dataset_id.table_id`
```

### 探索時は LIMIT を付ける

```sql
SELECT * FROM `project.dataset.table` LIMIT 100
```

### コスト最適化

- `SELECT *` を避け、必要なカラムのみ指定
- WHERE でスキャン範囲を限定
- パーティションフィルタを活用

## エラー対応

| エラー | 原因 | 対処 |
|--------|------|------|
| Not found | データセット/テーブル名の誤り | list_dataset_ids/list_table_ids で確認 |
| Access denied | 権限不足 | BigQuery Data Viewer ロールを確認 |
| Query timeout | クエリが複雑 | LIMIT 追加、パーティション活用 |
| Column not found | カラム名の誤り | get_table_info でスキーマ確認 |
