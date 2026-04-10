---
name: check-azure-retirements
description: Azure の廃止予定サービスやリタイアメント情報を確認する。サービスの EOL、非推奨、廃止スケジュールを調べるときに使用する。Use when checking Azure service retirements, deprecations, or end-of-life schedules.
---

# Azure 廃止予定情報の確認

Azure サービスの廃止 (Retirement) 情報を取得して、影響を受けるサービスと対応期限を確認する。

## 手順

1. 以下のコマンドを実行して廃止予定の更新情報を取得する:

```bash
FILTER="tags/any(t: t eq 'Retirements')"
curl -s "https://www.microsoft.com/releasecommunications/api/v2/azure/?\$top=20&\$filter=${FILTER}&\$orderby=modified%20desc" | python3 -c "
import sys, json
data = json.load(sys.stdin)
for item in data.get('value', []):
    retirements = [f\"{a.get('month','')}/{a.get('year','')}\" for a in item.get('availabilities', []) if a.get('ring') == 'Retirement']
    print(f\"Title: {item['title']}\")
    print(f\"Products: {', '.join(item.get('products', []))}\")
    print(f\"Retirement Date: {', '.join(retirements) if retirements else 'N/A'}\")
    print(f\"Modified: {item.get('modified', 'N/A')}\")
    print(f\"URL: https://azure.microsoft.com/ja-jp/updates/{item['id']}\")
    print('---')
"
```

2. 結果をユーザーにわかりやすくまとめる:
   - 廃止予定のサービス名
   - 廃止予定日
   - 移行先の推奨サービス (`description` から抽出)
   - 関連リンク: `https://azure.microsoft.com/ja-jp/updates/{id}`

## 特定製品の廃止情報を確認する場合

```bash
PRODUCT="Azure Stream Analytics"
FILTER="tags/any(t: t eq 'Retirements') and products/any(p: p eq '${PRODUCT}')"
```

詳細な API リファレンスは [../search-azure-updates/REFERENCE.md](../search-azure-updates/REFERENCE.md) を参照。
