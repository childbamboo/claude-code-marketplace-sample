# General プラグイン

汎用開発ツール - MCP サーバー統合とドキュメント処理スキル

## 概要

generalプラグインは、開発ワークフローを効率化するための汎用的なツールとスキルを提供します。以下の2つの主要カテゴリに分かれています：

1. **MCPサーバー統合** - Context7、Serena、Cipher などの外部サービスとの連携
2. **ドキュメント処理スキル** - 各種ファイル形式の読み込み・変換

---

## 提供コマンド

### /general:onboarding

プロジェクトオンボーディングプロセスを実行します。新規参加者が迅速にプロジェクトを理解できるようサポートします。

---

## MCPサーバー統合

### Context7
最新ライブラリドキュメントの検索と取得

### Serena
セマンティックなコード分析・編集

### Cipher
記憶管理とコンテキスト保持

---

## ドキュメント処理スキル

generalプラグインは、以下のドキュメント処理スキルを提供します：

### 1. xlsx-reader
**Excel (.xlsx) ファイルを読み込んで Markdown 形式に変換**

- 複数シートの読み込み対応
- Markdown テーブル形式への変換
- シート指定・行数制限機能
- WSL環境での実行

**前提条件:**
```bash
wsl pip3 install openpyxl
```

**使用例:**
```bash
wsl python3 plugins/general/skills/xlsx-reader/scripts/read_xlsx.py "/mnt/c/path/to/file.xlsx"
```

詳細: [xlsx-reader/README.md](./skills/xlsx-reader/README.md)

---

### 2. docx-reader
**Microsoft Word (.docx) ファイルのテキスト抽出**

- 段落テキストの抽出
- テーブルデータの抽出
- Markdown形式での保存

**前提条件:**
```bash
wsl pip3 install python-docx
```

**使用例:**
```bash
wsl python3 plugins/general/skills/docx-reader/scripts/read_docx.py "/mnt/c/path/to/file.docx"
```

詳細: [docx-reader/SKILL.md](./skills/docx-reader/SKILL.md)

---

### 3. pdf-reader
**PDF ファイルのテキスト抽出と Markdown 変換**

- ページごとのテキスト抽出
- テーブルの Markdown 化
- 複数ページの構造化

**前提条件:**
```bash
wsl pip3 install pdfplumber
```

**使用例:**
```bash
wsl python3 plugins/general/skills/pdf-reader/scripts/read_pdf.py "/mnt/c/path/to/file.pdf"
```

詳細: [pdf-reader/SKILL.md](./skills/pdf-reader/SKILL.md)

---

### 4. pdf-vision-reader
**図表が多い PDF を画像解析で Markdown 化**

- PDF を画像に変換
- Claude の vision 機能で解析
- 図表・グラフ・チャートの理解

**前提条件:**
```bash
wsl pip3 install pdf2image Pillow
wsl sudo apt-get install -y poppler-utils
```

**使用例:**
```bash
wsl python3 plugins/general/skills/pdf-vision-reader/scripts/pdf_to_images.py "/mnt/c/path/to/file.pdf"
```

詳細: [pdf-vision-reader/SKILL.md](./skills/pdf-vision-reader/SKILL.md)

---

### 5. jira-markdown-conversion
**Jira データを Markdown 形式に変換**

- Jira issue詳細の Markdown 化
- バックログのテーブル化
- Sprint情報のドキュメント化

**使用MCP:** Atlassian MCP

詳細: [jira-markdown-conversion/SKILL.md](./skills/jira-markdown-conversion/SKILL.md)

---

### 6. notion-markdown-conversion
**Notion データを Markdown 形式に変換**

- Notion database の Markdown 化
- ページ一覧のテーブル化
- Database schema の可視化

**使用MCP:** Notion MCP

詳細: [notion-markdown-conversion/SKILL.md](./skills/notion-markdown-conversion/SKILL.md)

---

## スキルの使い分けガイド

### ファイル形式別

| ファイル形式 | 推奨スキル | 備考 |
|-------------|----------|------|
| .xlsx (Excel) | **xlsx-reader** | 複数シート対応 |
| .docx (Word) | **docx-reader** | テキスト・テーブル抽出 |
| .pdf (テキスト中心) | **pdf-reader** | 高速テキスト抽出 |
| .pdf (図表・グラフ多) | **pdf-vision-reader** | Vision解析、処理時間長 |
| Jira データ | **jira-markdown-conversion** | Atlassian MCP必要 |
| Notion データ | **notion-markdown-conversion** | Notion MCP必要 |

### 用途別

| 用途 | 推奨スキル |
|------|----------|
| スプレッドシートデータの可視化 | xlsx-reader |
| 文書のテキスト抽出 | docx-reader, pdf-reader |
| プレゼン資料の解析 | pdf-vision-reader |
| タスク管理データの可視化 | jira-markdown-conversion |
| ナレッジベースの移行 | notion-markdown-conversion |

---

## トラブルシューティング

### WSLパス変換

Windows パスから WSL パスへの変換：
- `C:\Users\...` → `/mnt/c/Users/...`
- `D:\Projects\...` → `/mnt/d/Projects/...`

### Pythonパッケージのインストール

すべてのパッケージを一括インストール：
```bash
wsl pip3 install openpyxl python-docx pdfplumber pdf2image Pillow
wsl sudo apt-get install -y poppler-utils
```

### 個別パッケージのトラブル

- **openpyxl**: xlsx-reader に必要
- **python-docx**: docx-reader に必要（`docx` パッケージではない）
- **pdfplumber**: pdf-reader に必要
- **pdf2image + poppler-utils**: pdf-vision-reader に必要

---

## バージョン履歴

| バージョン | 日付 | 変更内容 |
|-----------|------|---------|
| 1.1.0 | 2026-01-08 | ドキュメント処理スキルを統合（xlsx/docx/pdf-reader, jira/notion-conversion） |
| 1.0.0 | 2026-01-06 | 初期リリース - MCP統合とonboardingコマンド |

---

## ライセンス

MIT License
