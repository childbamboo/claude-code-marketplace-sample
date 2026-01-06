# Claude Code プラグインマーケットプレイス

このリポジトリは、Claude Code のプラグインを自社内で配布するためのマーケットプレイスです。

## Claude Code のインストール

Claude Code を初めて使用する場合は、インストールと認証を行ってください。

### インストール

#### npm を使用する場合
```bash
npm install -g @anthropic-ai/claude-code
```

#### Homebrew を使用する場合（macOS/Linux）
```bash
brew install anthropic/claude-code/claude-code
```

インストールの確認：

```bash
claude --version
```

### 認証設定

```bash
claude auth login
```

ブラウザが開き、Anthropic アカウントでログインします。

認証の確認：

```bash
claude auth status
```

## マーケットプレイスの使い方

### マーケットプレイスの追加

```bash
# ローカルディレクトリから追加
/plugin marketplace add ./path/to/claude-code-marketplace-sample

# GitHub リポジトリから追加
/plugin marketplace add {owner}/claude-code-marketplace-sample
```

### プラグインのインストール

```bash
# 特定のメンバーのプラグインをインストール
/plugin install {plugin-name}@{marketplace-name}

# 例: sample メンバーのプラグインをインストール
/plugin install sample-plugin@claude-code-marketplace-sample
```

### 利用可能なプラグインの確認

```bash
/plugin
```

対話的なメニューから "Browse Plugins" を選択してください。

## 利用可能なプラグイン

### backend
バックエンド開発用ツール - API作成、マイグレーション、セキュリティ監査

**含まれるコマンド:**
- `/create-api` - API エンドポイントを作成
- `/create-migration` - データベースマイグレーションを作成
- `/generate-tests` - テストコードを生成
- `/audit-security` - セキュリティ監査を実施

**含まれるエージェント:**
- `api-builder` - API エンドポイント構築専門エージェント
- `db-architect` - データベーススキーマ設計専門エージェント
- `security-auditor` - セキュリティ監査専門エージェント

**インストール:**
```bash
/plugin install backend@claude-code-marketplace-sample
```

### frontend
フロントエンド開発用ツール - コンポーネント作成、テスト生成、コードレビュー

**含まれるコマンド:**
- `/create-component` - UI コンポーネントを作成
- `/generate-tests` - テストコードを生成
- `/review-code` - コードレビューを実施
- `/run-tests` - テストを実行

**含まれるエージェント:**
- `component-builder` - UI コンポーネント作成専門エージェント
- `test-generator` - フロントエンドテスト生成専門エージェント

**インストール:**
```bash
/plugin install frontend@claude-code-marketplace-sample
```

### spec
仕様書管理用ツール - 仕様書の作成、レビュー、更新

**含まれるコマンド:**
- `/create` - 仕様書を作成
- `/review` - 仕様書をレビュー
- `/update` - 仕様書を更新

**含まれるエージェント:**
- `spec-writer` - 仕様書作成専門エージェント
- `spec-reviewer` - 仕様書レビュー専門エージェント

**インストール:**
```bash
/plugin install spec@claude-code-marketplace-sample
```

### general
汎用開発ツール - Context7、Serena、Cipher の MCP サーバー統合

**含まれるコマンド:**
- `/onboarding` - プロジェクトオンボーディング

**含まれる MCP サーバー:**
- `context7` - ライブラリドキュメント検索
- `serena` - セマンティックコード分析
- `cipher` - メモリ管理

**インストール:**
```bash
/plugin install general@claude-code-marketplace-sample
```

## ディレクトリ構造

```
claude-code-marketplace-sample/
├── .claude-plugin/
│   └── marketplace.json          # マーケットプレイス定義
├── plugins/                       # プラグイン格納ディレクトリ
│   ├── sample/                    # サンプルプラグイン
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json       # プラグインマニフェスト
│   │   ├── commands/              # カスタムコマンド
│   │   │   └── hello.md
│   │   └── agents/                # サブエージェント
│   │       └── code-reviewer.md
│   ├── backend/                   # バックエンド開発用プラグイン
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── create-api.md
│   │   │   ├── create-migration.md
│   │   │   ├── generate-tests.md
│   │   │   └── audit-security.md
│   │   └── agents/
│   │       ├── api-builder.md
│   │       ├── db-architect.md
│   │       └── security-auditor.md
│   ├── frontend/                  # フロントエンド開発用プラグイン
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── create-component.md
│   │   │   ├── generate-tests.md
│   │   │   ├── review-code.md
│   │   │   └── run-tests.md
│   │   └── agents/
│   │       ├── component-builder.md
│   │       └── test-generator.md
│   ├── spec/                      # 仕様書管理用プラグイン
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── create-spec.md
│   │   │   ├── review-spec.md
│   │   │   └── update-spec.md
│   │   └── agents/
│   │       ├── spec-writer.md
│   │       └── spec-reviewer.md
│   └── {plugin_name}/             # あなたのプラグイン
│       └── ...
├── templates/                     # 旧形式（非推奨）
├── CLAUDE.md                      # リポジトリ固有の指示
└── README.md                      # このファイル
```

## プラグインの追加方法

新しいプラグインを追加する場合は、以下の手順に従ってください：

### 1. プラグイン用ディレクトリの作成

```bash
mkdir -p plugins/{plugin_name}
```

### 2. プラグインマニフェストの作成

`plugins/{plugin_name}/.claude-plugin/plugin.json` を作成：

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "あなたの名前",
    "email": "your-email@example.com"
  }
}
```

### 3. コマンドまたはエージェントの追加

**コマンドの例** (`plugins/{plugin_name}/commands/hello.md`):

```markdown
---
description: Hello World を出力
---

"Hello World" と出力してください。
```

**エージェントの例** (`plugins/{plugin_name}/agents/reviewer.md`):

```markdown
---
name: code-reviewer
description: コードレビューを実施
tools: Read, Grep, Glob
model: inherit
---

あなたはコードレビューの専門家です。コードの品質、セキュリティ、保守性を確認してください。
```

### 4. マーケットプレイスへの登録

`.claude-plugin/marketplace.json` の `plugins` 配列に追加：

```json
{
  "name": "plugin-name",
  "source": "./plugins/{plugin_name}",
  "description": "プラグインの説明",
  "version": "1.0.0"
}
```

## 参考資料

- [Plugins](https://code.claude.com/docs/en/plugins)
- [Plugins Reference](https://code.claude.com/docs/en/plugins-reference)
- [Plugin Marketplaces](https://code.claude.com/docs/en/plugin-marketplaces)
- [スラッシュコマンド](https://code.claude.com/docs/ja/slash-commands)
- [サブエージェント](https://code.claude.com/docs/ja/sub-agents)
- [MCP](https://code.claude.com/docs/ja/mcp)
- [フックガイド](https://code.claude.com/docs/ja/hooks-guide)
