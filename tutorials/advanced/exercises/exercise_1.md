# 演習1: プロジェクト管理データベースの作成と操作

## 目的
この演習では、プロジェクト管理用のデータベースを作成し、基本的なCRUD操作を実践します。

## 課題内容

### 課題1: データベースの作成
1. 以下の構造を持つプロジェクト管理データベースを作成してください：

プロパティ一覧：
- タイトル（プロジェクト名）
- 状態（計画中、進行中、完了）
- 優先度（高、中、低）
- 開始日
- 期限
- 担当者
- 進捗率（0-100%）

#### 解答例
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-database</tool_name>
<arguments>
{
  "parent": {
    "type": "page_id",
    "page_id": "親ページのID"
  },
  "title": [
    {
      "type": "text",
      "text": {
        "content": "プロジェクト管理"
      }
    }
  ],
  "properties": {
    "プロジェクト名": {
      "title": {}
    },
    "状態": {
      "select": {
        "options": [
          {
            "name": "計画中",
            "color": "blue"
          },
          {
            "name": "進行中",
            "color": "green"
          },
          {
            "name": "完了",
            "color": "purple"
          }
        ]
      }
    },
    "優先度": {
      "select": {
        "options": [
          {
            "name": "高",
            "color": "red"
          },
          {
            "name": "中",
            "color": "yellow"
          },
          {
            "name": "低",
            "color": "gray"
          }
        ]
      }
    },
    "開始日": {
      "date": {}
    },
    "期限": {
      "date": {}
    },
    "担当者": {
      "people": {}
    },
    "進捗率": {
      "number": {
        "format": "percent"
      }
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 課題2: データの追加
1. 以下のプロジェクトをデータベースに追加してください：

プロジェクト1:
- 名称: "ウェブサイトリニューアル"
- 状態: "進行中"
- 優先度: "高"
- 開始日: "2025-04-01"
- 期限: "2025-06-30"
- 進捗率: 30

プロジェクト2:
- 名称: "モバイルアプリ開発"
- 状態: "計画中"
- 優先度: "中"
- 開始日: "2025-07-01"
- 期限: "2025-12-31"
- 進捗率: 0

#### 解答例
```typescript
// プロジェクト1の追加
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
    },
    "優先度": {
      "select": {
        "name": "高"
      }
    },
    "開始日": {
      "date": {
        "start": "2025-04-01"
      }
    },
    "期限": {
      "date": {
        "start": "2025-06-30"
      }
    },
    "進捗率": {
      "number": 30
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 課題3: データの検索と更新
1. 進行中のプロジェクトを検索
2. 期限が2025年内のプロジェクトを優先度順に表示
3. ウェブサイトリニューアルの進捗率を40%に更新

#### 解答例
```typescript
// 進行中のプロジェクト検索
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

// 2025年内期限のプロジェクトを優先度順に表示
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
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## 評価ポイント
1. データベース設計
   - 適切なプロパティタイプの選択
   - オプション値の設定
   - 必要な情報の網羅

2. データ操作
   - 正確なデータ入力
   - 適切なフィルタリング
   - 効率的な更新処理

3. エラー処理
   - 入力値の検証
   - エラー発生時の対応
   - データの整合性確保

## ヒント
- データベース作成時はプロパティの型を慎重に選択
- 日付フィールドはタイムゾーンを考慮
- 複合フィルターを活用して効率的な検索を実現

## 発展課題
1. プロジェクトの依存関係を表現するリレーション追加
2. ステータス変更時の自動通知機能
3. 進捗レポートの自動生成

## 次のステップ
この演習が完了したら、[演習2](exercise_2.md)に進んで、より高度なデータベース操作を学びましょう。
