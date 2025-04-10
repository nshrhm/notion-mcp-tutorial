# 演習2: データベースとページ作成

## 目的
この演習では、Notionデータベースの操作とページ作成の実践的な使用方法を学びます。

## 課題内容

### 課題1: データベースの検索と詳細確認
1. ワークスペース内の "Projects" という名前のデータベースを検索し、そのIDを取得してください。
2. 取得したデータベースIDを使用して、データベースの詳細情報（特に `properties` の定義）を取得・確認してください。これは次の課題で正しいプロパティ名と型を使うために重要です。

#### 解答例
```typescript
// 1. データベース検索
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "Projects",
  "filter": {
    "property": "object",
    "value": "database"
  },
  "page_size": 1 // 通常、名前が一致するDBは少ないため1件取得で十分
}
</arguments>
</use_mcp_tool>

// 検索結果からデータベースIDを取得 (例: searchResults は上記ツールの結果)
// const databaseId = searchResults.results[0]?.id;
// if (!databaseId) { throw new Error("データベースが見つかりません"); }

// 2. データベース詳細情報の取得
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-a-database</tool_name>
<arguments>
{
  // "database_id": databaseId // 上で取得したIDを設定
  "database_id": "検索で見つけたデータベースID"
}
</arguments>
</use_mcp_tool>

// 上記ツールの結果から properties の内容を確認する
```

### 課題2: データベースへのページ追加
1. 課題1で見つけたデータベースに新しいプロジェクトページを作成してください。
2. **重要:** 以下の解答例にあるプロパティ名（`Name`, `Start Date`, `Status`, `Assignee`）と型（`title`, `date`, `select`, `people`）は、**あなたの「Projects」データベースの実際のプロパティ定義に合わせて修正する必要があります。** 課題1で取得した情報やNotion UIで確認してください。
3. ページには最低限、以下の情報を含めてください（プロパティ名は適宜修正）：
   - プロジェクト名 (タイトルプロパティ)
   - 開始日 (日付プロパティ)
   - 状態 (セレクトプロパティ、例: "進行中")
   - 担当者 (ユーザープロパティ)

#### 解答例
**注意:** 以下の `properties` 内の名前と型は、ご自身のデータベースに合わせてください。
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
1. 課題2で作成したページに、以下のプロパティ情報を追加または更新してください。
2. **重要:** 以下の解答例にあるプロパティ名（`Description`, `Tags`）と型（`rich_text`, `multi_select`）は、**あなたの「Projects」データベースの実際のプロパティ定義に合わせて修正する必要があります。**
   - プロジェクト説明 (リッチテキストプロパティ)
   - タグ (マルチセレクトプロパティ)
3. 更新内容に関するコメントをページに追加してください。

#### 解答例
**注意:** 以下の `properties` 内の名前と型は、ご自身のデータベースに合わせてください。
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
