---
description: テストコードを生成
---

# テストコード生成（バックエンド）

既存のAPIエンドポイント、サービス、関数に対するテストコードを自動生成します。

## 生成対象

テストを生成したい対象を指定してください：

- APIエンドポイント（ルート/コントローラー）
- サービス層の関数
- ビジネスロジック
- ユーティリティ関数
- データベースモデル

## 生成されるテストの種類

### 1. ユニットテスト
- 関数の入出力テスト
- ビジネスロジックの検証
- エッジケースのテスト
- エラーハンドリング
- バリデーションロジック

### 2. 統合テスト
- APIエンドポイントのテスト
- データベース操作のテスト
- 外部サービス連携のテスト
- トランザクション処理

### 3. E2Eテスト
- ユーザーシナリオのテスト
- 複数のエンドポイントを跨ぐフロー
- 認証・認可のテスト

## テストフレームワーク

対応するフレームワーク：

### Node.js / JavaScript / TypeScript
- **Jest**
- **Mocha** + **Chai**
- **Vitest**
- **Supertest** (APIテスト)

### Python
- **pytest**
- **unittest**
- **FastAPI TestClient**

### Go
- **testing** (標準ライブラリ)
- **testify**

## 生成内容例

### APIエンドポイントテスト
```typescript
describe('POST /api/users', () => {
  it('creates a new user with valid data', async () => {
    // 正常系テスト
  });

  it('returns 400 when email is invalid', async () => {
    // バリデーションエラーテスト
  });

  it('returns 401 when not authenticated', async () => {
    // 認証エラーテスト
  });
});
```

### サービス層テスト
```typescript
describe('UserService', () => {
  it('creates user and sends welcome email', async () => {
    // ビジネスロジックテスト
  });

  it('throws error when email already exists', async () => {
    // エラーケーステスト
  });
});
```

## モック・スタブ生成

以下のモックを自動生成します：

- データベースモック
- 外部APIモック
- メール送信モック
- ファイルシステムモック

## ベストプラクティス適用

- テストの独立性確保
- Setup/Teardown の実装
- テストデータのファクトリー
- トランザクションのロールバック
- 環境変数の分離

テストを生成したい対象を教えてください。
