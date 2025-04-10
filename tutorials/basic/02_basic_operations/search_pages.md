# Notionページの検索

## 概要
このガイドでは、Notion APIを使用してページとデータベースを検索する方法を説明します。検索機能は利用可能なAPIの中核機能の1つです。

## 基本的な検索

### 1. シンプルな検索
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "sort": {
    "direction": "descending",
    "timestamp": "last_edited_time"
  },
  "page_size": 10
}
</arguments>
</use_mcp_tool>
```

### 2. フィルター付き検索
   - `filter` パラメータを使って、検索対象をページ (`page`) またはデータベース (`database`) に限定できます。
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "filter": {
    "property": "object", // フィルター対象のプロパティ (現在は "object" のみサポート)
    "value": "page"      // フィルター値 ("page" または "database")
  }
}
</arguments>
</use_mcp_tool>
```

### 3. ソート順の変更
   - `sort` パラメータで、結果の並び順を変更できます。`last_edited_time` (最終更新日時) に基づく昇順 (`ascending`) または降順 (`descending`) でソート可能です。
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "sort": {
    "direction": "ascending",      // "ascending" または "descending"
    "timestamp": "last_edited_time" // 現在は "last_edited_time" のみサポート
  }
}
</arguments>
</use_mcp_tool>
```

## 検索結果の処理

### 1. 基本的な結果処理
```javascript
// 検索結果の例
{
  "object": "list",
  "results": [
    {
      "object": "page",
      "id": "ページID",
      "properties": {
        "title": { ... }
      }
    }
  ],
  "next_cursor": "次のページのカーソル",
  "has_more": true
}
```

### 2. ページネーション処理
```typescript
// 次のページを取得
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "start_cursor": "前回のnext_cursor",
  "page_size": 10
}
</arguments>
</use_mcp_tool>
```

## エラーハンドリング

### 1. 一般的なエラー
- 検索クエリが無効
- ページサイズの制限超過
- 権限エラー

### 2. エラー処理例
```javascript
try {
  // 検索APIの呼び出し
} catch (error) {
  if (error.code === "invalid_request") {
    console.error("検索クエリが無効です");
  } else if (error.code === "unauthorized") {
    console.error("アクセス権限がありません");
  }
}
```

## ベストプラクティス

### 1. 効率的な検索
- 具体的な検索クエリの使用
- 適切なフィルターの適用
- 必要最小限のページサイズ指定

### 2. パフォーマンスの最適化
- 検索結果のキャッシュ
- ページネーションの活用
- バッチ処理の実装

## よくある質問

### Q: 検索結果が0件の場合
- クエリの正確性を確認
- フィルター条件を見直し
- アクセス権限を確認

### Q: 検索が遅い場合
- ページサイズを調整
- フィルターを最適化
- キャッシュの活用を検討

## 次のステップ
検索操作を理解したら、[ページ情報の取得](get_page.md)に進んでください。
