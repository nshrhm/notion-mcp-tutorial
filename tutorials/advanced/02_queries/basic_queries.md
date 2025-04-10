# データベースのクエリとフィルタリング

## はじめに
このチュートリアルでは、Notionデータベースからデータを取得し、フィルタリングやソートを行う方法を学びます。

## データベースのクエリ基本

### 1. 基本的なクエリ
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "page_size": 10
}
</arguments>
</use_mcp_tool>
```

### 2. フィルター付きクエリ
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "filter": {
    "property": "状態",
    "select": {
      "equals": "進行中"
    }
  }
}
</arguments>
</use_mcp_tool>
```

## フィルタリングオプション

### 1. プロパティタイプ別のフィルター

#### テキストフィルター
```json
{
  "property": "タイトル",
  "rich_text": {
    "contains": "プロジェクト"
  }
}
```

#### 数値フィルター
```json
{
  "property": "進捗率",
  "number": {
    "greater_than_or_equal_to": 50
  }
}
```

#### 日付フィルター
```json
{
  "property": "期限",
  "date": {
    "before": "2025-12-31"
  }
}
```

#### 選択肢フィルター
```json
{
  "property": "優先度",
  "select": {
    "equals": "高"
  }
}
```

### 2. 複合フィルター

#### AND条件
```json
{
  "and": [
    {
      "property": "状態",
      "select": {
        "equals": "進行中"
      }
    },
    {
      "property": "優先度",
      "select": {
        "equals": "高"
      }
    }
  ]
}
```

#### OR条件
```json
{
  "or": [
    {
      "property": "状態",
      "select": {
        "equals": "計画中"
      }
    },
    {
      "property": "状態",
      "select": {
        "equals": "進行中"
      }
    }
  ]
}
```

## ソートオプション

### 1. 単一プロパティでのソート
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "sorts": [
    {
      "property": "期限",
      "direction": "ascending"
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 2. 複数プロパティでのソート
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "sorts": [
    {
      "property": "優先度",
      "direction": "descending"
    },
    {
      "property": "期限",
      "direction": "ascending"
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## ページネーション

### 1. 基本的なページネーション
```typescript
// 最初のページを取得
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "page_size": 5
}
</arguments>
</use_mcp_tool>

// 次のページを取得（start_cursorを使用）
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "start_cursor": "前回のレスポンスのnext_cursor",
  "page_size": 5
}
</arguments>
</use_mcp_tool>
```

## レスポンスの扱い方

### 1. 基本的なレスポンス構造
```json
{
  "object": "list",
  "results": [
    {
      "object": "page",
      "id": "ページID",
      "properties": {
        "タイトル": { ... },
        "状態": { ... },
        "期限": { ... }
      }
    }
  ],
  "next_cursor": "次のページのカーソル",
  "has_more": true
}
```

### 2. プロパティ値の取得
```javascript
// タイトルの取得
const title = results[0].properties.タイトル.title[0].text.content;

// 選択値の取得
const status = results[0].properties.状態.select.name;

// 日付の取得
const dueDate = results[0].properties.期限.date.start;
```

## エラーハンドリング

### よくあるエラーと対処方法

1. フィルター関連
   - プロパティ名の確認
   - フィルター条件の形式確認
   - 値の型の確認

2. ソート関連
   - ソート可能なプロパティタイプの確認
   - ソート方向の指定確認

3. ページネーション関連
   - カーソルの有効性確認
   - ページサイズの制限確認

## ベストプラクティス

1. クエリの最適化
   - 必要な情報のみを取得
   - 適切なページサイズの設定
   - キャッシュの活用検討

2. エラー処理
   - エラーケースの想定
   - 適切なエラーメッセージの提供
   - リトライ戦略の実装

3. パフォーマンス
   - クエリの複雑さを考慮
   - 結果セットのサイズ管理
   - バッチ処理の検討

## 演習問題
1. 特定の条件に合うタスクの検索
2. 複数の条件でのフィルタリング
3. ソートとページネーションの組み合わせ

## 次のステップ
クエリの基本を理解したら、[高度なデータベース操作](../advanced_operations.md)に進んで、より複雑な操作方法を学びましょう。
