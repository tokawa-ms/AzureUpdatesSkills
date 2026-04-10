# Azure Updates API リファレンス

## エンドポイント

- **ベース URL**: `https://www.microsoft.com/releasecommunications/api/v2/azure/`
- **RSS フィード**: `https://www.microsoft.com/releasecommunications/api/v2/azure/rss`

## クエリパラメータ

| パラメータ | 説明                       | 例                             |
| ---------- | -------------------------- | ------------------------------ |
| `$top`     | 取得件数の上限             | `$top=10`                      |
| `$skip`    | ページネーションオフセット | `$skip=100`                    |
| `$filter`  | OData フィルター式         | `$filter=status eq 'Launched'` |
| `$search`  | 全文検索                   | `$search=kubernetes`           |
| `$orderby` | ソート                     | `$orderby=modified desc`       |

## レスポンススキーマ

| フィールド                       | 型          | 説明                           |
| -------------------------------- | ----------- | ------------------------------ |
| `id`                             | string      | 一意な識別子 (スラグ形式)      |
| `title`                          | string      | 更新タイトル                   |
| `description`                    | string      | HTML 形式の説明                |
| `status`                         | string/null | `Launched`, `In preview`, null |
| `productCategories`              | string[]    | 製品カテゴリ                   |
| `tags`                           | string[]    | タグ                           |
| `products`                       | string[]    | 対象製品                       |
| `generalAvailabilityDate`        | string/null | GA 日 (`YYYY-MM`)              |
| `previewAvailabilityDate`        | string/null | プレビュー開始日               |
| `privatePreviewAvailabilityDate` | string/null | プライベートプレビュー開始日   |
| `created`                        | string      | 作成日時 (ISO 8601)            |
| `modified`                       | string      | 更新日時 (ISO 8601)            |
| `availabilities`                 | object[]    | `ring`, `year`, `month` を含む |

## 既知の値一覧

### productCategories

- AI + machine learning
- Analytics
- Compute
- Containers
- Databases
- Developer tools
- DevOps
- Hybrid + multicloud
- Integration
- Internet of Things
- Management and governance
- Migration
- Mobile
- Networking
- Security
- Storage
- Web

### tags

- Compliance
- Features
- Management
- Microsoft Build
- Microsoft Ignite
- Open Source
- Pricing & Offerings
- Regions & Datacenters
- Retirements
- SDK and Tools
- Security
- Services

### status

- `Launched` (一般提供開始)
- `In preview` (プレビュー中)
- null (廃止予定)

### availabilities.ring

- `General Availability`
- `Preview`
- `Retirement`
- `Private Preview`

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

## フィルター式の例

```
# ステータスでフィルタ
$filter=status eq 'Launched'
$filter=status eq 'In preview'

# タイトルで部分一致検索
$filter=contains(title,'Cosmos')

# 製品カテゴリでフィルタ
$filter=productCategories/any(c: c eq 'Databases')

# タグでフィルタ
$filter=tags/any(t: t eq 'Retirements')

# 製品でフィルタ
$filter=products/any(p: p eq 'Azure Kubernetes Service (AKS)')

# 複合条件
$filter=status eq 'Launched' and productCategories/any(c: c eq 'Databases')
```
