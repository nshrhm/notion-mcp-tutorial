# 演習2: データベースとページ作成

## 目的
この演習では、Notionデータベースの操作とページ作成の実践的な使用方法を学びます。

## 課題内容

### 課題1: データベースの検索
1. ワークスペース内の "Projects" という名前のデータベースを検索してください
2. データベースの詳細情報を取得してください

#### 解答例
```typescript
// データベース検索
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "Projects",
  "filter": {
    "property": "object",
    "value": "database"
  }
}
</arguments>
</use_mcp_tool>
```

### 課題2: データベースへのページ追加
1. 見つけたデータベースに新しいプロジェクトページを作成してください
2. 以下の情報を含めてください：
   - プロジェクト名
   - 開始日
   - 状態（進行中）
   - 担当者

#### 解答例
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-page</tool_name>
<arguments>
{
  "parent": {
    "database_id": "データベースID"
  },
  "properties": {
    "Name": {
      "title": [
        {
          "text": {
            "content": "新規プロジェクト2025"
          }
        }
      ]
    },
    "Start Date": {
      "date": {
        "start": "2025-04-09"
      }
    },
    "Status": {
      "select": {
        "name": "進行中"
      }
    },
    "Assignee": {
      "people": [
        {
          "id": "ユーザーID"
        }
      ]
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 課題3: ページの更新とコメント追加
1. 作成したページに以下の更新を行ってください：
   - プロジェクト説明の追加
   - タグの追加
2. 更新内容に関するコメントを追加してください

#### 解答例
```typescript
// ページの更新
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-patch-page</tool_name>
<arguments>
{
  "page_id": "作成したページID",
  "properties": {
    "Description": {
      "rich_text": [
        {
          "text": {
            "content": "このプロジェクトは..."
          }
        }
      ]
    },
    "Tags": {
      "multi_select": [
        {
          "name": "優先度高"
        },
        {
          "name": "開発"
        }
      ]
    }
  }
}
</arguments>
</use_mcp_tool>

// コメントの追加
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "作成したページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "プロジェクト情報を更新しました。\n- プロジェクト説明を追加\n- 優先度とカテゴリタグを設定"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## 評価ポイント
1. データベース操作
   - 正しい検索フィルターの使用
   - データベース構造の理解

2. ページ作成
   - 必要なプロパティの設定
   - 適切なデータ型の使用

3. 更新とコメント
   - 既存ページの適切な更新
   - 分かりやすいコメントの追加

## ヒント
- データベースのプロパティ構造を事前に確認することが重要です
- プロパティの型に応じて適切な形式でデータを設定してください
- 更新操作は既存のデータを保持したまま部分的な変更が可能です

## 発展課題
1. 複数のページを一括で作成
2. データベースのビューをプログラムで操作
3. 定期的な更新を自動化する方法の検討

## 解答の提出方法
1. 各APIコールの結果を記録
2. エラーが発生した場合の対処方法を記述
3. 発展課題に取り組んだ場合はその結果も含める

## 次のステップ
この演習が完了したら、実際のプロジェクトでの活用方法を検討しましょう。
Notion APIを使用した効率的なワークフローの構築に挑戦してください。
