# 演習1の解答例と解説

## データベース作成の解説

### 実装のポイント
1. プロパティの型選択
   - タイトル: 必須プロパティとして設定
   - 状態: 選択肢を限定し、色分けで視認性向上
   - 日付: タイムゾーンを考慮した設定
   - 進捗率: パーセント表示の数値型

2. データ型の特徴
   - select: 一貫性のある値管理が可能
   - number: 計算や比較が容易
   - date: 期間管理とカレンダー表示に対応
   - people: ユーザー管理との連携

### コード解説
```typescript
// データベース作成時の重要なポイント
{
  "properties": {
    "プロジェクト名": {
      "title": {}  // データベースには必ずtitleプロパティが必要
    },
    "状態": {
      "select": {
        "options": [
          {
            "name": "計画中",
            "color": "blue"  // 色による視覚的な状態管理
          }
        ]
      }
    },
    "進捗率": {
      "number": {
        "format": "percent"  // パーセント表示で進捗を分かりやすく
      }
    }
  }
}
```

## データ追加の実装例

### プロジェクト追加の詳細手順

1. 基本データの設定
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-page</tool_name>
<arguments>
{
  "parent": {
    "database_id": "データベースID"
  },
  "properties": {
    "プロジェクト名": {
      "title": [
        {
          "text": {
            "content": "ウェブサイトリニューアル"
          }
        }
      ]
    },
    "状態": {
      "select": {
        "name": "進行中"
      }
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
              "content": "企業Webサイトのリニューアルプロジェクト"
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

2. 追加データの確認
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-page</tool_name>
<arguments>
{
  "page_id": "作成したページID"
}
</arguments>
</use_mcp_tool>
```

## 検索と更新の実装

### 効率的な検索の実現方法

1. 複合フィルターの活用
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "filter": {
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
}
</arguments>
</use_mcp_tool>
```

2. ソートとフィルターの組み合わせ
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-query-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "filter": {
    "property": "期限",
    "date": {
      "before": "2026-01-01"
    }
  },
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

### 効果的な更新処理

1. プロパティの部分更新
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-update-page</tool_name>
<arguments>
{
  "page_id": "ページID",
  "properties": {
    "進捗率": {
      "number": 40
    }
  }
}
</arguments>
</use_mcp_tool>
```

## ベストプラクティス

1. データ整合性の維持
   - 更新前の値の確認
   - トランザクション的な更新の実装
   - エラー時のロールバック考慮

2. パフォーマンス最適化
   - 必要なプロパティのみ取得
   - バッチ処理の活用
   - キャッシュの検討

3. エラーハンドリング
   - API制限の考慮
   - ネットワークエラーへの対応
   - 再試行メカニズムの実装

## 発展的な実装例

### 自動更新システム
```typescript
// 進捗率に基づくステータス自動更新
async function updateProjectStatus(projectId, progress) {
  if (progress >= 100) {
    <use_mcp_tool>
    <server_name>notionApi</server_name>
    <tool_name>API-update-page</tool_name>
    <arguments>
    {
      "page_id": projectId,
      "properties": {
        "状態": {
          "select": {
            "name": "完了"
          }
        }
      }
    }
    </arguments>
    </use_mcp_tool>
  }
}
```

### バッチ処理の実装
```typescript
// 複数プロジェクトの一括更新
async function batchUpdateProjects(projects) {
  for (const project of projects) {
    <use_mcp_tool>
    <server_name>notionApi</server_name>
    <tool_name>API-update-page</tool_name>
    <arguments>
    {
      "page_id": project.id,
      "properties": {
        "進捗率": {
          "number": project.progress
        }
      }
    }
    </arguments>
    </use_mcp_tool>
  }
}
```

## 次のステップ
この基本的な実装を理由解したら、[演習2](../exercise_2.md)で学ぶリレーションとワークフロー自動化に進みましょう。
