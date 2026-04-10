<p align="center">
  <img src="https://img.icons8.com/fluency/96/azure-1.png" alt="Azure Updates Skills" width="96" />
</p>

<h1 align="center">Azure Updates Skills for GitHub Copilot</h1>

<p align="center">
  <strong>Custom Skills that enable GitHub Copilot Agent to search Azure Updates via natural language</strong>
</p>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT" /></a>
  <a href="https://github.com/copilot"><img src="https://img.shields.io/badge/GitHub%20Copilot-Skills-blue?logo=githubcopilot" alt="GitHub Copilot Skills" /></a>
  <a href="https://azure.microsoft.com/en-us/updates"><img src="https://img.shields.io/badge/Azure-Updates-0078D4?logo=microsoftazure" alt="Azure Updates" /></a>
  <a href="./README.md"><img src="https://img.shields.io/badge/lang-日本語-green" alt="日本語" /></a>
</p>

---

## 📖 Overview

**Azure Updates Skills** is a skill set that leverages [GitHub Copilot](https://github.com/features/copilot) custom skills to search and retrieve [Azure Updates](https://azure.microsoft.com/en-us/updates) information directly from Copilot Chat using natural language.

Stay informed about Azure new feature releases, preview announcements, and service retirements — all without leaving your editor.

## ✨ Features

| Skill                                                                                             | Description                                                                                                 |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 🔍 **[search-azure-updates](.github/skills/search-azure-updates/SKILL.md)**                       | Search Azure Updates with natural language. Supports keywords, status filters, product categories, and more |
| 📋 **[get-latest-azure-updates](.github/skills/get-latest-azure-updates/SKILL.md)**               | Fetch and display the latest Azure Updates                                                                  |
| 🏷️ **[filter-azure-updates-by-product](.github/skills/filter-azure-updates-by-product/SKILL.md)** | Filter Azure Updates by product name or category                                                            |
| ⚠️ **[check-azure-retirements](.github/skills/check-azure-retirements/SKILL.md)**                 | Check Azure service retirement and deprecation schedules                                                    |

## 🏗️ Architecture

```
AzureUpdatesSkills/
├── .github/
│   ├── copilot-instructions.md          # Global instructions for Copilot
│   └── skills/
│       ├── search-azure-updates/        # Natural language search skill
│       │   ├── SKILL.md
│       │   └── REFERENCE.md             # API reference
│       ├── get-latest-azure-updates/    # Latest updates skill
│       │   └── SKILL.md
│       ├── filter-azure-updates-by-product/  # Product filter skill
│       │   └── SKILL.md
│       └── check-azure-retirements/     # Retirement check skill
│           └── SKILL.md
├── LICENSE
├── README.md
└── README-en.md
```

The skills use the REST API that powers the [Azure Updates](https://azure.microsoft.com/en-us/updates) web page. The API is accessible without authentication, so no API keys or secrets are needed.

> **⚠️ Note**: This API is **not** officially documented or supported by Microsoft. It was discovered by reverse-engineering the Azure Updates web page backend. Endpoints, response schemas, and available parameters may change or be removed without notice.

## 🚀 Setup

### Prerequisites

- [GitHub Copilot](https://github.com/features/copilot) (Business / Enterprise / Individual)
- [Visual Studio Code](https://code.visualstudio.com/) with the [GitHub Copilot extension](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
- `curl` and `python3` available on your PATH

### Installation

1. Clone or fork this repository:

   ```bash
   git clone https://github.com/<your-username>/AzureUpdatesSkills.git
   ```

2. Open the folder in VS Code:

   ```bash
   code AzureUpdatesSkills
   ```

3. Open GitHub Copilot Chat — the skills are automatically recognized. The SKILL.md files under `.github/skills/` are loaded by Copilot, making the commands available immediately.

## 💬 Usage Examples

### Get the latest Azure Updates

> "Show me the latest Azure updates"

### Search by keyword

> "Search Azure Updates for Kubernetes"

### Filter by product category

> "Show me the latest updates in the Databases category"

### Check retirements

> "Check if Azure Stream Analytics has any retirement plans"

### Filter by status

> "List Azure services that recently went into preview"

## 🔌 API Details

This project uses the REST API that powers the [Azure Updates](https://azure.microsoft.com/en-us/updates) web page.

| Property       | Value                                                              |
| -------------- | ------------------------------------------------------------------ |
| Base URL       | `https://www.microsoft.com/releasecommunications/api/v2/azure/`    |
| RSS Feed       | `https://www.microsoft.com/releasecommunications/api/v2/azure/rss` |
| Authentication | None required                                                      |
| Protocol       | OData v4 compatible                                                |

> **⚠️ Disclaimer**: This API is **not** officially documented or supported by Microsoft. It was discovered by reverse-engineering the Azure Updates web page backend. Endpoints, response formats, and available parameters may change or be removed without notice. This is an unofficial community project and is not affiliated with Microsoft.

For a detailed API reference, see [REFERENCE.md](.github/skills/search-azure-updates/REFERENCE.md).

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/amazing-skill`)
3. Commit your changes (`git commit -m 'Add amazing skill'`)
4. Push to the branch (`git push origin feature/amazing-skill`)
5. Open a Pull Request

## 📄 License

This project is licensed under the [MIT License](LICENSE).

Copyright (c) 2025 Takashi Okawa

## 🔗 Related Links

- [Azure Updates Official Site](https://azure.microsoft.com/en-us/updates)
- [GitHub Copilot](https://github.com/features/copilot)
- [GitHub Copilot Custom Skills Documentation](https://docs.github.com/copilot)
