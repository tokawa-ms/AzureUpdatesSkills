---
name: get-latest-azure-updates
description: Azure Updates の最新情報を取得する。最近の Azure の変更、新機能、更新一覧を知りたいときに使用する。Use when fetching the latest Azure Updates or recent changes.
---

# Azure Updates 最新情報取得

最新の Azure Updates を取得して表示する。

## 手順

1. 以下のコマンドを実行して最新の Azure Updates を取得する:

```bash
curl -s "https://www.microsoft.com/releasecommunications/api/v2/azure/?\$top=20&\$orderby=modified%20desc" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for item in data.get('value', []):
    print(f\"Title: {item['title']}\")
    print(f\"Status: {item.get('status', 'N/A')}\")
    print(f\"Products: {', '.join(item.get('products', []))}\")
    print(f\"Categories: {', '.join(item.get('productCategories', []))}\")
    print(f\"Tags: {', '.join(item.get('tags', []))}\")
    print(f\"GA Date: {item.get('generalAvailabilityDate', 'N/A')}\")
    print(f\"Preview Date: {item.get('previewAvailabilityDate', 'N/A')}\")
    print(f\"Modified: {item.get('modified', 'N/A')}\")
    print(f\"URL: https://azure.microsoft.com/ja-jp/updates/{item['id']}\")
    print('---')
"
```

2. 結果をユーザーにわかりやすくまとめる:
   - 各更新のタイトル、ステータス、対象製品、公開日を表示
   - ステータスに応じてグループ化 (GA, Preview, Retirement)
   - 関連リンク: `https://azure.microsoft.com/ja-jp/updates/{id}`
