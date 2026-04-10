# Azure Updates Search Skill

このリポジトリには、Azure Updates (https://azure.microsoft.com/ja-jp/updates) の情報を自然言語で検索するための Agent Skills が含まれています。

## Azure Updates API 仕様

- **ベース URL**: `https://www.microsoft.com/releasecommunications/api/v2/azure/`
- **プロトコル**: OData v4 準拠の REST API
- **RSS フィード**: `https://www.microsoft.com/releasecommunications/api/v2/azure/rss`

### 対応クエリパラメータ

| パラメータ | 説明                       | 例                             |
| ---------- | -------------------------- | ------------------------------ |
| `$top`     | 取得件数の上限             | `$top=10`                      |
| `$skip`    | ページネーションオフセット | `$skip=100`                    |
| `$filter`  | OData フィルター式         | `$filter=status eq 'Launched'` |
| `$search`  | 全文検索                   | `$search=kubernetes`           |
| `$orderby` | ソート                     | `$orderby=modified desc`       |

### レスポンススキーマ

各アイテムのフィールド:

| フィールド                       | 型          | 説明                                        |
| -------------------------------- | ----------- | ------------------------------------------- |
| `id`                             | string      | 一意な識別子 (スラグ形式)                   |
| `title`                          | string      | 更新タイトル                                |
| `description`                    | string      | HTML 形式の説明                             |
| `status`                         | string/null | `Launched`, `In preview`, null (廃止の場合) |
| `productCategories`              | string[]    | 製品カテゴリ                                |
| `tags`                           | string[]    | タグ                                        |
| `products`                       | string[]    | 対象製品                                    |
| `generalAvailabilityDate`        | string/null | GA 日 (`YYYY-MM` 形式)                      |
| `previewAvailabilityDate`        | string/null | プレビュー開始日                            |
| `privatePreviewAvailabilityDate` | string/null | プライベートプレビュー開始日                |
| `created`                        | string      | 作成日時 (ISO 8601)                         |
| `modified`                       | string      | 更新日時 (ISO 8601)                         |
| `availabilities`                 | object[]    | 利用可能性情報 (`ring`, `year`, `month`)    |

### 既知の productCategories 一覧

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

### 既知の tags 一覧

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

### 既知の status 値

- `Launched` (一般提供開始)
- `In preview` (プレビュー中)
- null (廃止予定)

### 既知の availabilities.ring 値

- `General Availability`
- `Preview`
- `Retirement`

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
