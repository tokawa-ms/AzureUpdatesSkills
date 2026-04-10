<p align="center">
  <img src="https://img.icons8.com/fluency/96/azure-1.png" alt="Azure Updates Skills" width="96" />
</p>

<h1 align="center">Azure Updates Skills for GitHub Copilot</h1>

<p align="center">
  <strong>GitHub Copilot Agent に Azure Updates の検索能力を付与する Custom Skills</strong>
</p>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT" /></a>
  <a href="https://github.com/copilot"><img src="https://img.shields.io/badge/GitHub%20Copilot-Skills-blue?logo=githubcopilot" alt="GitHub Copilot Skills" /></a>
  <a href="https://azure.microsoft.com/ja-jp/updates"><img src="https://img.shields.io/badge/Azure-Updates-0078D4?logo=microsoftazure" alt="Azure Updates" /></a>
  <a href="./README-en.md"><img src="https://img.shields.io/badge/lang-English-green" alt="English" /></a>
</p>

---

## 📖 概要

**Azure Updates Skills** は、[GitHub Copilot](https://github.com/features/copilot) のカスタムスキル機能を活用して、[Azure Updates](https://azure.microsoft.com/ja-jp/updates) の情報をチャットから自然言語で検索・取得できるようにするスキルセットです。

Azure の新機能リリース、プレビュー開始、サービス廃止 (Retirement) などの最新情報を、ブラウザを開かずに Copilot Chat から直接確認できます。

## ✨ 主な機能

| スキル                                                                                            | 説明                                                                                      |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 🔍 **[search-azure-updates](.github/skills/search-azure-updates/SKILL.md)**                       | 自然言語で Azure Updates を検索。キーワード、ステータス、製品カテゴリなど多様な条件に対応 |
| 📋 **[get-latest-azure-updates](.github/skills/get-latest-azure-updates/SKILL.md)**               | 最新の Azure Updates 一覧を取得して表示                                                   |
| 🏷️ **[filter-azure-updates-by-product](.github/skills/filter-azure-updates-by-product/SKILL.md)** | 製品名やカテゴリで Azure Updates をフィルタリング                                         |
| ⚠️ **[check-azure-retirements](.github/skills/check-azure-retirements/SKILL.md)**                 | Azure サービスの廃止・リタイアメント情報を確認                                            |

## 🏗️ アーキテクチャ

```
AzureUpdatesSkills/
├── .github/
│   ├── copilot-instructions.md          # Copilot へのグローバル指示
│   └── skills/
│       ├── search-azure-updates/        # 自然言語検索スキル
│       │   ├── SKILL.md
│       │   └── REFERENCE.md             # API リファレンス
│       ├── get-latest-azure-updates/    # 最新情報取得スキル
│       │   └── SKILL.md
│       ├── filter-azure-updates-by-product/  # 製品別フィルタスキル
│       │   └── SKILL.md
│       └── check-azure-retirements/     # 廃止情報確認スキル
│           └── SKILL.md
├── LICENSE
├── README.md
└── README-en.md
```

スキルは [Azure Updates](https://azure.microsoft.com/ja-jp/updates) ページのバックエンドで使用されている REST API を利用しています。認証なしでアクセス可能なため、API キーやシークレットの設定は不要です。

> **⚠️ 注意**: この API は Microsoft が公式にドキュメント化・サポートしているものではありません。Azure Updates Web ページのバックエンドを解析して利用しているため、予告なくエンドポイントやスキーマが変更される可能性があります。

## 🚀 セットアップ

### 前提条件

- [GitHub Copilot](https://github.com/features/copilot) (Business / Enterprise / Individual)
- [Visual Studio Code](https://code.visualstudio.com/) + [GitHub Copilot 拡張機能](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)
- `curl` と `python3` がパス上で利用可能であること

### インストール

1. このリポジトリをクローンまたはフォークする:

   ```bash
   git clone https://github.com/<your-username>/AzureUpdatesSkills.git
   ```

2. VS Code でフォルダを開く:

   ```bash
   code AzureUpdatesSkills
   ```

3. GitHub Copilot Chat を開き、スキルが自動的に認識されます。`.github/skills/` 配下の SKILL.md が Copilot に読み込まれ、対応するコマンドが利用可能になります。

## 💬 使用例

### 最新の Azure Updates を確認

> 「最近の Azure の更新情報を教えて」

### キーワードで検索

> 「Kubernetes に関する Azure Updates を検索して」

### 製品カテゴリでフィルタ

> 「データベースカテゴリの最新アップデートを見せて」

### 廃止予定の確認

> 「Azure Stream Analytics の廃止予定を確認して」

### ステータスでフィルタ

> 「最近プレビューになった Azure サービスを一覧表示して」

## 🔌 API について

このプロジェクトは [Azure Updates](https://azure.microsoft.com/ja-jp/updates) Web ページのバックエンドで使用されている REST API を利用しています。

| 項目       | 値                                                                 |
| ---------- | ------------------------------------------------------------------ |
| Base URL   | `https://www.microsoft.com/releasecommunications/api/v2/azure/`    |
| RSS Feed   | `https://www.microsoft.com/releasecommunications/api/v2/azure/rss` |
| 認証       | 不要                                                               |
| プロトコル | OData v4 互換                                                      |

> **⚠️ 免責事項**: この API は Microsoft により公式にドキュメント化・サポートされているものではありません。Azure Updates Web ページの内部 API をリバースエンジニアリングにより解析して利用しています。そのため、エンドポイント、レスポンス形式、利用可能なパラメータ等が予告なく変更・廃止される可能性があります。本プロジェクトは Microsoft とは一切関係のない非公式のコミュニティプロジェクトです。

詳細な API リファレンスは [REFERENCE.md](.github/skills/search-azure-updates/REFERENCE.md) を参照してください。

## 🤝 コントリビューション

コントリビューションを歓迎します！以下の方法で貢献できます:

1. このリポジトリをフォーク
2. フィーチャーブランチを作成 (`git checkout -b feature/amazing-skill`)
3. 変更をコミット (`git commit -m 'Add amazing skill'`)
4. ブランチにプッシュ (`git push origin feature/amazing-skill`)
5. Pull Request を作成

## 📄 ライセンス

このプロジェクトは [MIT License](LICENSE) の下で公開されています。

Copyright (c) 2026 Takashi Okawa

## 🔗 関連リンク

- [Azure Updates 公式サイト](https://azure.microsoft.com/ja-jp/updates)
- [GitHub Copilot](https://github.com/features/copilot)
- [GitHub Copilot Custom Skills ドキュメント](https://docs.github.com/copilot)
