---
name: artifact-charts
description: "データ分析結果を高速にグラフ化。アーティファクトで棒グラフ、折れ線グラフ、円グラフを即座に描画。可視化、グラフ作成、チャート描画時に使用。"
---

# 高速グラフ描画

データを受け取ったら、以下のテンプレートにデータを差し込んでアーティファクトを生成する。

## ポリシー: シンプルさを最優先

- **1 リクエスト = 1〜2 グラフ**: ダッシュボードは作らない
- **装飾は最小限**: アニメーション、グラデーション、影は使わない
- **凡例・ラベルは必要な場合のみ**: データが自明なら省略
- **色は単色 or 3 色まで**: 視認性を優先
- **インタラクティブ機能は不要**: ホバー、ズーム、フィルタは追加しない

**禁止事項:**
- 複数グラフを並べたダッシュボード
- タブ切り替え UI
- リアルタイム更新機能
- 外部データ読み込み

## グラフ選択

| データの性質 | グラフ | 例 |
|-------------|--------|-----|
| カテゴリ比較 | 棒グラフ | カテゴリ別売上 |
| 時系列推移 | 折れ線 | 月別推移 |
| 構成比 | 円グラフ | 売上構成比 |

## 棒グラフ

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <canvas id="chart"></canvas>
  <script>
    new Chart(document.getElementById('chart'), {
      type: 'bar',
      data: {
        labels: ['A', 'B', 'C'],  // ← ラベル
        datasets: [{
          label: '売上',
          data: [100, 200, 150],  // ← 値
          backgroundColor: '#4F46E5'
        }]
      }
    });
  </script>
</body>
</html>
```

## 折れ線グラフ

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <canvas id="chart"></canvas>
  <script>
    new Chart(document.getElementById('chart'), {
      type: 'line',
      data: {
        labels: ['1月', '2月', '3月'],  // ← ラベル
        datasets: [{
          label: '売上推移',
          data: [100, 150, 200],  // ← 値
          borderColor: '#4F46E5',
          tension: 0.1
        }]
      }
    });
  </script>
</body>
</html>
```

## 円グラフ

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <canvas id="chart"></canvas>
  <script>
    new Chart(document.getElementById('chart'), {
      type: 'pie',
      data: {
        labels: ['A', 'B', 'C'],  // ← ラベル
        datasets: [{
          data: [40, 35, 25],  // ← 値
          backgroundColor: ['#4F46E5', '#10B981', '#F59E0B']
        }]
      }
    });
  </script>
</body>
</html>
```

## 使い方

1. データの性質からグラフ種類を選択
2. テンプレートの `labels` と `data` を差し替え
3. アーティファクトとして出力
