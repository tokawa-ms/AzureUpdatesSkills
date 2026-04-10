---
name: search-azure-updates
description: Azure Updates を自然言語で検索する。ユーザーが Azure の更新情報、新機能、プレビュー、GA、廃止について質問したときに使用する。Use when searching Azure Updates, finding new features, preview announcements, GA releases, or retirements.
---

# Azure Updates 検索

ユーザーの自然言語クエリに基づいて Azure Updates API からデータを取得し、結果を表示する。

## API 概要

- **ベース URL**: `https://www.microsoft.com/releasecommunications/api/v2/azure/`
- **プロトコル**: OData v4 準拠 REST API
- **クエリパラメータ**: `$top`, `$skip`, `$filter`, `$search`, `$orderby`

## 検索手順

1. ユーザーの入力を解析し、以下を判断する:
   - **キーワード**: `$search` パラメータに使用 (英語で検索)
   - **ステータスフィルタ**: `Launched` / `In preview` / 廃止 (null)
   - **製品カテゴリ**: `productCategories/any(c: c eq '...')`
   - **製品名**: `products/any(p: p eq '...')`
   - **タグ**: `tags/any(t: t eq '...')`
   - **期間**: `generalAvailabilityDate`, `created`, `modified` を利用
   - **件数**: デフォルト 10 件

2. API を呼び出す:

```bash
BASE_URL="https://www.microsoft.com/releasecommunications/api/v2/azure/"
# パラメータを組み立てる ($search, $filter, $orderby, $top)
curl -s "${BASE_URL}?\$top=10&\$orderby=modified%20desc&\$search=kubernetes" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for item in data.get('value', []):
    print(f\"Title: {item['title']}\")
    print(f\"Status: {item.get('status', 'N/A')}\")
    print(f\"Products: {', '.join(item.get('products', []))}\")
    print(f\"GA Date: {item.get('generalAvailabilityDate', 'N/A')}\")
    print(f\"Modified: {item.get('modified', 'N/A')}\")
    print(f\"URL: https://azure.microsoft.com/ja-jp/updates/{item['id']}\")
    print('---')
"
```

3. 結果をユーザーにわかりやすくまとめる:
   - 各更新情報のタイトル、ステータス、対象製品、公開日を表示
   - `description` は HTML なのでプレーンテキストに要約
   - 関連 URL: `https://azure.microsoft.com/ja-jp/updates/{id}`

## フィルタ構築ルール

### ステータスの自然言語マッピング

| ユーザー入力                                | フィルタ                                  |
| ------------------------------------------- | ----------------------------------------- |
| GA / 一般提供 / リリース済み / ローンチ済み | `$filter=status eq 'Launched'`            |
| プレビュー / パブリックプレビュー           | `$filter=status eq 'In preview'`          |
| 廃止 / リタイア / 終了                      | `$filter=tags/any(t: t eq 'Retirements')` |

### 製品カテゴリの自然言語マッピング

| ユーザー入力                  | カテゴリ値                  |
| ----------------------------- | --------------------------- |
| AI / 機械学習                 | `AI + machine learning`     |
| 分析                          | `Analytics`                 |
| コンピューティング / VM       | `Compute`                   |
| コンテナ / K8s                | `Containers`                |
| データベース / DB             | `Databases`                 |
| 開発ツール                    | `Developer tools`           |
| DevOps / CI/CD                | `DevOps`                    |
| ハイブリッド / マルチクラウド | `Hybrid + multicloud`       |
| 統合                          | `Integration`               |
| IoT                           | `Internet of Things`        |
| 管理 / ガバナンス             | `Management and governance` |
| 移行 / マイグレーション       | `Migration`                 |
| モバイル                      | `Mobile`                    |
| ネットワーク                  | `Networking`                |
| セキュリティ                  | `Security`                  |
| ストレージ                    | `Storage`                   |
| Web / ウェブ                  | `Web`                       |

## 注意事項

- `$search` は英語で検索すること (API は英語データのみ)
- `description` フィールドは HTML 形式。API 応答はそのまま表示せず要約すること
- ページネーションには `$skip` を `$top` の倍数で増やす
- 最新順に取得するには `$orderby=modified desc`

詳細な API リファレンスは [REFERENCE.md](REFERENCE.md) を参照。
