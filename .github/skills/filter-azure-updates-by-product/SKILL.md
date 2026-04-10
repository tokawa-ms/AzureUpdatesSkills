---
name: filter-azure-updates-by-product
description: 特定の Azure 製品やサービスの更新情報を取得する。製品名やカテゴリで Azure Updates をフィルタリングして検索するときに使用する。Use when filtering Azure Updates by product name or category.
---

# Azure 製品別更新情報

特定の Azure 製品やサービスに関する更新情報をフィルタリングして取得する。

## 手順

1. ユーザーが指定した製品名/カテゴリから OData フィルタを構築する
2. API を呼び出す:

```bash
# 製品名でフィルタする例
PRODUCT="Azure Kubernetes Service (AKS)"
FILTER="products/any(p: p eq '${PRODUCT}')"
curl -s "https://www.microsoft.com/releasecommunications/api/v2/azure/?\$top=10&\$filter=${FILTER}&\$orderby=modified%20desc" | python3 -c "
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

## よく使われる製品名

- Azure Kubernetes Service (AKS)
- Azure Cosmos DB
- Azure Functions
- Azure SQL Database
- Azure SQL Managed Instance
- App Service
- Azure Container Apps
- Azure Monitor
- Azure Database for PostgreSQL
- Azure Database for MySQL
- Azure Front Door
- Azure Firewall
- Application Gateway
- Web Application Firewall
- Load Balancer
- Virtual Network
- Azure Blob Storage
- Azure NetApp Files
- Azure Databricks
- Azure Machine Learning
- API Management
- Logic Apps
- Service Bus
- Azure Stream Analytics
- Azure Chaos Studio
- Azure Arc
- Azure Data Box
- Azure Migrate

## カテゴリ別フィルタ

製品カテゴリでフィルタする場合:

```bash
CATEGORY="Databases"
FILTER="productCategories/any(c: c eq '${CATEGORY}')"
```

## 複合条件

ステータスとカテゴリの組み合わせ:

```bash
FILTER="status eq 'Launched' and productCategories/any(c: c eq 'Databases')"
```

詳細な API リファレンスは [../search-azure-updates/REFERENCE.md](../search-azure-updates/REFERENCE.md) を参照。
