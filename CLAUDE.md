やり取りは日本語で行うこと。

# Claude Bootstrap Kit (CBK) マーケットプレイス

このリポジトリは、**Claude Code のプラグインを社内で開発・配布するためのマーケットプレイス**です。開発効率を向上させる各種ツールとエージェントを提供します。

---

## 📋 目次

- [プロジェクト概要](#プロジェクト概要)
- [クイックスタート](#クイックスタート)
- [プラグイン構造](#プラグイン構造)
- [開発フロー](#開発フロー)
- [既存プラグイン](#既存プラグイン)
- [トラブルシューティング](#トラブルシューティング)
- [FAQ](#faq)
- [参考資料](#参考資料)

---

## プロジェクト概要

### 目的

- 社内メンバー間でClaude Codeプラグインを共有
- 開発ワークフローの標準化と効率化
- チーム固有のベストプラクティスの共有

### リポジトリ構成

```
claude-code-marketplace-sample/
├── .claude-plugin/
│   └── marketplace.json          # マーケットプレイス設定（プラグイン一覧）
├── plugins/                      # プラグイン格納ディレクトリ
│   ├── backend/                  # バックエンド開発ツール
│   ├── frontend/                 # フロントエンド開発ツール
│   ├── spec/                     # 仕様書管理ツール
│   └── general/                  # 汎用開発ツール（MCP統合）
├── data/                         # 共有データ（テンプレート、設定など）
├── CLAUDE.md                     # このファイル（プロジェクトコンテキスト）
├── PLUGIN_DEVELOPMENT.md         # プラグイン開発の詳細ガイドライン
└── README.md                     # リポジトリの概要
```

---

## クイックスタート

### 1. 環境セットアップ

```bash
# リポジトリをクローン
git clone <repository-url>
cd claude-code-marketplace-sample

# Claude Codeでプラグインを読み込み
# Claude Code の設定でこのディレクトリを指定
```

### 2. 既存プラグインの確認

```bash
# マーケットプレイスに登録されているプラグインを確認
cat .claude-plugin/marketplace.json

# 特定のプラグインの詳細を確認
cat plugins/backend/.claude-plugin/plugin.json
```

### 3. プラグインの使用

Claude Code で以下のようにプラグインのコマンドを実行できます：

```bash
# バックエンドAPI作成
/backend:create-api users

# フロントエンドコンポーネント作成
/frontend:create-component Button

# 仕様書作成
/spec:create api-specification.md
```

---

## プラグイン構造

### 標準ディレクトリレイアウト

各プラグインは以下の構造に従います：

```
plugins/{plugin_name}/
├── .claude-plugin/
│   └── plugin.json               # プラグインマニフェスト（必須）
├── commands/                     # カスタムスラッシュコマンド（オプション）
│   ├── command1.md
│   └── command2.md
├── agents/                       # サブエージェント（オプション）
│   ├── agent1.md
│   └── agent2.md
├── skills/                       # スキル（オプション）
│   └── skill1.md
└── README.md                     # プラグイン説明（推奨）
```

### plugin.json の基本構造

```json
{
  "name": "your-plugin",
  "version": "1.0.0",
  "description": "プラグインの簡潔な説明",
  "author": {
    "name": "あなたの名前"
  },
  "keywords": ["keyword1", "keyword2"],
  "dependencies": {
    "mcpServers": ["serena"]
  }
}
```

### コマンドファイルの基本構造

コマンドは YAML フロントマター付き Markdown で作成します：

```markdown
---
description: コマンドの簡潔な説明（60文字以内）
category: workflow
complexity: standard
---

# /your-command - コマンド名

## 核となる機能

このコマンドが何をするのかを2-3文で説明します。

## コマンド構文

\`\`\`
/your-command [arg1] [--option]
\`\`\`

## 実行フロー

### 1. **フェーズ名**: 目的
- 具体的なアクション
- 実行内容
```

### エージェントファイルの基本構造

エージェントも YAML フロントマター付き Markdown で作成します：

```markdown
---
description: エージェントの簡潔な説明（60文字以内）
tools: [Read, Write, Edit, Grep, Glob, Bash]
model: sonnet
category: specialist
---

# [Role] - エージェント名

## ペルソナ

あなたは[専門分野]の専門家です。

## 核となる責任

1. **責任1**: 詳細な説明
2. **責任2**: 詳細な説明
```

---

## 開発フロー

### 新しいプラグインを作成する場合

#### ステップ1: プラグインディレクトリの作成

```bash
# プラグイン名でディレクトリを作成
mkdir -p plugins/{plugin_name}/.claude-plugin
mkdir -p plugins/{plugin_name}/commands
mkdir -p plugins/{plugin_name}/agents
```

#### ステップ2: plugin.json の作成

```bash
# plugin.json を作成
cat > plugins/{plugin_name}/.claude-plugin/plugin.json << 'EOF'
{
  "name": "your-plugin",
  "version": "1.0.0",
  "description": "プラグインの説明",
  "author": {
    "name": "Your Name"
  },
  "keywords": ["keyword1", "keyword2"]
}
EOF
```

#### ステップ3: コマンドまたはエージェントの作成

詳細は [PLUGIN_DEVELOPMENT.md](./PLUGIN_DEVELOPMENT.md) を参照してください。

**重要**: コマンドとエージェントは以下の点に注意して作成してください：

- **YAMLフロントマター**を必ず含める
- **description**は60文字以内で明確に
- **実行フロー**を段階的に説明する
- **動作境界**（実行すること/しないこと）を明記する

#### ステップ4: marketplace.json への登録

```bash
# .claude-plugin/marketplace.json の plugins 配列に追加
# 以下のフォーマットで追加してください：
{
  "name": "your-plugin",
  "source": "./plugins/{plugin_name}",
  "description": "プラグインの説明",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  },
  "keywords": ["keyword1", "keyword2"]
}
```

#### ステップ5: テストと検証

```bash
# Claude Code でプラグインが正しく読み込まれるかテスト
# コマンドを実行して動作確認
/your-plugin:your-command

# 品質チェックリストを確認（PLUGIN_DEVELOPMENT.md参照）
```

#### ステップ6: プルリクエストの作成

```bash
# 変更をコミット
git add .
git commit -m "feat: add your-plugin"

# プルリクエストを作成
git push origin your-branch
```

### 既存プラグインを修正する場合

1. 該当プラグインのディレクトリに移動
2. 必要なファイルを編集
3. **必ずテスト**してから commit
4. plugin.json の version を更新（semantic versioning に従う）
5. プルリクエストを作成

---

## 既存プラグイン

### backend

**バックエンド開発用ツール** - API作成、マイグレーション、セキュリティ監査

**提供コマンド:**
- `/backend:create-api` - REST API エンドポイントを作成
- `/backend:create-migration` - データベースマイグレーションを作成
- `/backend:audit-security` - セキュリティ監査を実施
- `/backend:generate-tests` - テストコードを生成

**提供エージェント:**
- `backend-api-builder` - API構築専門家
- `backend-db-architect` - データベース設計専門家
- `backend-security-auditor` - セキュリティ監査専門家

### frontend

**フロントエンド開発用ツール** - コンポーネント作成、テスト生成、コードレビュー

**提供コマンド:**
- `/frontend:create-component` - Reactコンポーネントを作成
- `/frontend:generate-tests` - テストコードを生成
- `/frontend:review-code` - コードレビューを実施
- `/frontend:run-tests` - テスト実行とカバレッジ確認

**提供エージェント:**
- `frontend-component-builder` - コンポーネント作成専門家
- `frontend-test-generator` - テスト生成専門家

### spec

**仕様書管理用ツール** - 仕様書の作成、レビュー、更新

**提供コマンド:**
- `/spec:create` - 仕様書を作成
- `/spec:review` - 仕様書をレビュー
- `/spec:update` - 仕様書を更新

**提供エージェント:**
- `spec-writer` - 仕様書作成専門家
- `spec-reviewer` - 仕様書レビュー専門家

### general

**汎用開発ツール** - Context7、Serena、Cipher の MCP サーバー統合

**提供コマンド:**
- `/general:onboarding` - プロジェクトオンボーディング

**MCPサーバー統合:**
- **Context7**: 最新ライブラリドキュメントの検索
- **Serena**: セマンティックなコード分析・編集
- **Cipher**: 記憶管理とコンテキスト保持

---

## トラブルシューティング

### プラグインが読み込まれない

**原因**: plugin.json の形式が正しくない

**解決方法**:
```bash
# JSONの構文チェック
cat plugins/{plugin_name}/.claude-plugin/plugin.json | jq .

# エラーがある場合は修正
```

### コマンドが認識されない

**原因**: YAMLフロントマターの形式が正しくない、またはファイル名が正しくない

**解決方法**:
- YAMLフロントマターが `---` で囲まれているか確認
- `description` フィールドが存在するか確認
- ファイル名が kebab-case になっているか確認
- ファイル拡張子が `.md` になっているか確認

### エージェントが起動しない

**原因**: tools フィールドが正しく設定されていない

**解決方法**:
```markdown
---
description: エージェントの説明
tools: [Read, Write, Edit, Grep, Glob, Bash]  # 必須
model: sonnet  # 必須
---
```

### マーケットプレイスに表示されない

**原因**: marketplace.json に登録されていない

**解決方法**:
```bash
# marketplace.json の plugins 配列を確認
cat .claude-plugin/marketplace.json | jq '.plugins'

# 登録されていなければ追加
```

### コマンドを実行するとエラーが出る

**原因**: コマンドのプロンプト内でツールの使い方が間違っている、または必要なMCPサーバーが起動していない

**解決方法**:
- コマンドファイルのプロンプト内容を確認
- 必要なMCPサーバーが起動しているか確認
- Task tool を使う場合は subagent_type を正しく指定しているか確認

---

## FAQ

### Q1: プラグイン名の命名規則は？

**A**: シンプルで分かりやすい名前を推奨します。例: `backend`, `frontend`, `devops`

### Q2: 複数のメンバーで同じプラグインを開発できますか？

**A**: はい。`plugins/` 配下のディレクトリ名は自由に設定できます。チーム名を使用することも推奨します。

### Q3: プラグインの version はいつ更新すべきですか？

**A**: Semantic Versioning に従ってください：
- **MAJOR** (1.0.0 → 2.0.0): 後方互換性のない変更
- **MINOR** (1.0.0 → 1.1.0): 後方互換性のある機能追加
- **PATCH** (1.0.0 → 1.0.1): 後方互換性のあるバグ修正

### Q4: MCPサーバーの設定方法は？

**A**: plugin.json の dependencies フィールドで指定します：
```json
{
  "dependencies": {
    "mcpServers": ["serena", "context7", "cipher"]
  }
}
```

### Q5: コマンドとエージェントの違いは？

**A**:
- **コマンド**: ユーザーが `/command-name` で呼び出す、特定のワークフローを実行するプロンプト
- **エージェント**: Task tool で呼び出される、特定のドメインに特化した専門家ペルソナ

### Q6: カスタムメタデータフィールド（category, complexity など）は必須ですか？

**A**: 必須ではありませんが、**強く推奨**します。Claude Code は無視しますが、ドキュメント目的および将来的なツール活用のために有用です。

### Q7: プラグインのテスト方法は？

**A**:
1. Claude Code でプラグインを読み込む
2. コマンドを実際に実行して動作確認
3. エージェントを Task tool で呼び出して動作確認
4. エッジケース（異常系）をテスト
5. PLUGIN_DEVELOPMENT.md の品質チェックリストを確認

### Q8: プラグイン開発で参考にすべきリソースは？

**A**:
- [PLUGIN_DEVELOPMENT.md](./PLUGIN_DEVELOPMENT.md) - 詳細な設計ガイドライン
- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework) - 設計哲学の参考
- 既存プラグイン (`plugins/` 配下) - 実装例
- Claude Code 公式ドキュメント

---

## 参考資料

### 詳細ドキュメント

- **[PLUGIN_DEVELOPMENT.md](./PLUGIN_DEVELOPMENT.md)** - プラグイン開発の詳細ガイドライン（必読）
  - 設計原則
  - コマンド/エージェント設計パターン
  - ベストプラクティス
  - 品質基準
  - テンプレート

### 内部ドキュメント

- `.claude-plugin/marketplace.json` - プラグイン一覧
- `plugins/*/README.md` - 各プラグインの説明

### 外部リソース

- [Claude Code Documentation](https://docs.claude.com) - 公式ドキュメント
- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework) - 設計哲学の参考元

---

## 貢献ガイドライン

新しいプラグインを追加する場合や既存プラグインを改善する場合は、以下を遵守してください：

1. **[PLUGIN_DEVELOPMENT.md](./PLUGIN_DEVELOPMENT.md) を必ず読む**
2. **品質チェックリスト**を確認してから commit
3. **テスト**を必ず実施
4. **プルリクエスト**を作成してレビューを受ける

---

## ライセンス

MIT License

---

## 更新履歴

| 日付 | バージョン | 変更内容 |
|------|-----------|---------|
| 2025-01-06 | 2.0.0 | 大幅なブラッシュアップ - クイックスタート、開発フロー、トラブルシューティング、FAQを追加 |
| 2025-12-09 | 1.0.0 | 初版作成 |
