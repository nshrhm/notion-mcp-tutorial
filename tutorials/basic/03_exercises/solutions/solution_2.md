# 演習2の解答例と解説

## 課題1: データベースの検索

### 基本的な解答例
```typescript
// データベースの検索
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

### 詳細解説
1. 検索パラメータ
   - `query`: データベース名を指定
   - `filter`: データベースのみを検索対象に
   - `object`: "database"を指定して種類を限定

2. 注意点
   - 大文字小文字は区別される
   - 部分一致も検索される
   - 複数のデータベースが見つかる可能性がある

## 課題2: データベースへのページ追加

### 基本的な解答例
**重要:** 以下の解答例にあるプロパティ名（`Name`, `Start Date`, `Status`, `Assignee`）と型は、**あなたの「Projects」データベースの実際のプロパティ定義に合わせて修正する必要があります。** 演習課題1で取得した情報やNotion UIで確認してください。
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
  },
  "children": [
    {
      "object": "block",
      "type": "heading_2",
      "heading_2": {
        "rich_text": [
          {
            "text": {
              "content": "プロジェクト概要"
            }
          }
        ]
      }
    },
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "rich_text": [
          {
            "text": {
              "content": "このプロジェクトは2025年度の主要施策として実施されます。"
            }
          }
        ]
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 詳細解説
1. プロパティの設定
   - `title`: ページのタイトル（必須）
   - `date`: 日付形式のプロパティ
   - `select`: 単一選択のプロパティ
   - `people`: ユーザー参照のプロパティ

2. コンテンツの追加
   - `children`: ページの本文内容
   - 見出しやパラグラフを構造化
   - リッチテキスト形式で記述

## 課題3: ページの更新とコメント追加

### 解答例
**重要:** 以下の解答例にあるプロパティ名（`Description`, `Tags`）と型は、**あなたの「Projects」データベースの実際のプロパティ定義に合わせて修正する必要があります。**
```typescript
// ページの更新
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-patch-page</tool_name>
<arguments>
{
  "page_id": "作成したページID",
  // propertiesパラメータはJSON文字列として渡す必要がある場合があるため、
  // ツール仕様を確認してください。ここではオブジェクトとして記述します。
  "properties": {
    "Description": {
      "rich_text": [
        {
          "text": {
            "content": "このプロジェクトは組織の中核を担う重要な取り組みです。"
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
        "content": "プロジェクト情報を更新しました。\n\n"
      }
    },
    {
      "text": {
        "content": "追加項目:\n"
      },
      "annotations": {
        "bold": true
      }
    },
    {
      "text": {
        "content": "- プロジェクト説明の詳細化\n- 優先度タグの設定\n- 開発カテゴリの追加\n\n"
      }
    },
    {
      "text": {
        "content": "次回のアップデートでマイルストーンを追加予定です。"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## 発展的な解答例

### 1. 複数ページの一括作成
```typescript
// 複数のプロジェクトページを一括作成
const projects = [
  {
    name: "プロジェクトA",
    startDate: "2025-04-10",
    status: "計画中"
  },
  {
    name: "プロジェクトB",
    startDate: "2025-05-01",
    status: "準備中"
  }
];

// 各プロジェクトに対してページを作成
projects.forEach(project => {
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
              "content": project.name
            }
          }
        ]
      },
      "Start Date": {
        "date": {
          "start": project.startDate
        }
      },
      "Status": {
        "select": {
          "name": project.status
        }
      }
    }
  }
  </arguments>
  </use_mcp_tool>
});
```

## ベストプラクティス

1. データベース操作
   - プロパティの型を正確に把握
   - 必須項目の確実な設定
   - 一意性の確保（重複確認）

2. ページ作成時の考慮点
   - 構造化されたコンテンツ設計
   - 必要な情報の網羅
   - 整合性のあるタグ付け

3. 更新操作の注意点
   - 既存データの保持
   - 部分更新の活用
   - 更新履歴の記録

4. エラーハンドリング
   - プロパティ型の検証
   - 必須値の確認
   - 権限の事前確認

## 実践的な応用例

1. 定期的な更新の自動化
   - 進捗状況の自動更新
   - マイルストーンのリマインド
   - ステータスレポートの生成

2. データの相互連携
   - 関連プロジェクトの紐付け
   - 依存関係の管理
   - クロスリファレンスの作成

3. レポーティング機能
   - 進捗サマリーの自動生成
   - KPIの追跡
   - タイムラインの可視化
