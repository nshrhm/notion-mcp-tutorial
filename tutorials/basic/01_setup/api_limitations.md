# Notion MCP API制限事項

## 利用可能なAPI機能

### 1. 基本機能
✓ 利用可能
- `API-get-self`: ユーザー情報の取得
- `API-post-search`: ページとデータベースの検索
- `API-create-a-comment`: コメントの追加

### 2. 制限された機能
⚠️ 現在利用できない
- データベース作成 API
- ページ作成 API
- データベース更新 API
- ブロック操作 API

## 推奨される代替アプローチ

### 1. データベース操作
1. Notion UI でデータベースを事前に作成
2. 検索APIを使用してデータベースにアクセス
3. コメント機能を使用して更新情報を記録

#### 実装例
```typescript
// データベースを検索
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "データベース名",
  "filter": {
    "property": "object",
    "value": "database"
  }
}
</arguments>
</use_mcp_tool>

// データベースIDを取得
const databaseId = results[0].id;

// コメントで情報を更新
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": databaseId
  },
  "rich_text": [
    {
      "text": {
        "content": "更新情報: 〇〇を変更しました"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 2. ページ管理
1. Notion UI で必要なページを事前に作成
2. 検索APIを使用してページを取得
3. コメントを活用して情報を追加

## ベストプラクティス

1. データ構造の設計
   - UI上で完全な構造を事前設定
   - 必要なプロパティやリレーションを準備
   - テンプレートページの活用

2. 情報管理
   - 検索機能を効果的に使用
   - コメントによる補足情報の追加
   - 構造化された情報の維持

3. エラー処理
   - API制限を考慮したフォールバック
   - 適切なエラーメッセージの提供
   - 代替手段の提示

## 更新予定

Notion APIの機能は継続的に更新される可能性があります。以下の機能の追加が期待されます：

1. データベース操作
   - データベース作成
   - プロパティの更新
   - レコードの追加・更新

2. ページ操作
   - ページ作成
   - コンテンツ更新
   - ブロック操作

## 開発者向けガイドライン

1. 実装の確認
   - 利用可能なAPIの確認
   - 機能の動作テスト
   - エラーケースの検証

2. ドキュメント管理
   - API仕様の定期確認
   - 制限事項の更新
   - ユーザーへの通知

3. エラーハンドリング
   - 適切なエラーメッセージ
   - 代替フローの提供
   - ユーザーガイダンス

## 次のステップ
現在の制限事項を理解した上で、利用可能な機能を最大限活用するアプローチを検討してください。必要に応じて、UIとAPIを組み合わせたハイブリッドなソリューションを検討することをお勧めします。
