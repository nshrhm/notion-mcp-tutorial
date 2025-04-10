# 演習2: 高度なデータベース操作とワークフロー自動化

## 目的
この演習では、データベース間のリレーション設定とワークフロー自動化の実践的な使用方法を学びます。

## 課題内容

### 課題1: リレーションデータベースの作成

1. 以下の2つのデータベースを作成してください：

#### プロジェクトデータベース（既存）
- プロジェクト名（タイトル）
- 状態
- 優先度
- 期限
- 担当チーム（リレーション）
- タスク一覧（リレーション）

#### チームデータベース（新規）
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-database</tool_name>
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
        "content": "チーム管理"
      }
    }
  ],
  "properties": {
    "チーム名": {
      "title": {}
    },
    "リーダー": {
      "people": {}
    },
    "メンバー": {
      "people": {}
    },
    "担当プロジェクト": {
      "relation": {
        "database_id": "プロジェクトデータベースID"
      }
    },
    "ステータス": {
      "select": {
        "options": [
          {
            "name": "稼働中",
            "color": "green"
          },
          {
            "name": "準備中",
            "color": "yellow"
          }
        ]
      }
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 課題2: リレーションの設定と更新

1. チームデータの追加：
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-page</tool_name>
<arguments>
{
  "parent": {
    "database_id": "チームデータベースID"
  },
  "properties": {
    "チーム名": {
      "title": [
        {
          "text": {
            "content": "開発チームA"
          }
        }
      ]
    },
    "ステータス": {
      "select": {
        "name": "稼働中"
      }
    }
  }
}
</arguments>
</use_mcp_tool>
```

2. プロジェクトとチームの関連付け：
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-patch-page</tool_name>
<arguments>
{
  "page_id": "プロジェクトのページID",
  // propertiesパラメータはJSON文字列として渡す必要がある場合があるため、
  // ツール仕様を確認してください。ここではオブジェクトとして記述します。
  "properties": {
    "担当チーム": {
      "relation": [
        {
          "id": "チームのページID"
        }
      ]
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 課題3: 自動更新ワークフローの作成

1. プロジェクトの進捗状況に基づく自動更新：
   - タスクの完了率が100%になったら、プロジェクトのステータスを「完了」に更新
   - 期限が近づいたプロジェクトにラベルを追加

```typescript
// プロジェクトの進捗確認と更新
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-database-query</tool_name>
<arguments>
{
  "database_id": "プロジェクトデータベースID",
  // フィルターオブジェクトをJSON文字列として渡す
  "filter": "{\"property\": \"進捗率\", \"number\": {\"equals\": 100}}"
}
</arguments>
</use_mcp_tool>

// 上記クエリの結果 (results) をループして、各ページのステータスを更新する想定
// for (const page of results) { ... }

// ステータス更新 (ループ内で実行するイメージ)
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-patch-page</tool_name>
<arguments>
{
  // "page_id": page.id, // ループ内のページID
  "page_id": "完了プロジェクトのID", // 例として単一IDを記載
  // propertiesパラメータはJSON文字列として渡す必要がある場合があるため、
  // ツール仕様を確認してください。ここではオブジェクトとして記述します。
  "properties": {
    "状態": {
      "select": {
        "name": "完了"
      }
    }
  }
  // properties をJSON文字列で渡す場合の例:
  // "properties": "{\"状態\": {\"select\": {\"name\": \"完了\"}}}"
}
</arguments>
</use_mcp_tool>
```
**注意:** 実際の自動更新ワークフローでは、データベースクエリの結果（完了したプロジェクトのリスト）を取得し、そのリスト内の各ページIDに対して更新処理 (`API-patch-page`) をループ実行する必要があります。上記の例は、更新処理の単体を示しています。

## 評価ポイント

1. データベース設計
   - 適切なリレーション構造
   - 効率的なデータ管理
   - 拡張性の考慮

2. リレーション管理
   - 正確な関連付け
   - 双方向の更新処理
   - 整合性の維持

3. 自動化ロジック
   - 条件分岐の適切さ
   - エラーハンドリング
   - パフォーマンスへの配慮

## ヒント
- リレーションは慎重に設計
- 自動更新は段階的にテスト
- エラー発生時のロールバックを考慮

## 発展課題
1. 複数データベース間の連携
   - プロジェクト → チーム → メンバー の階層構造
   - クロスリファレンス機能
   - 集計レポートの自動生成

2. ワークフロー自動化の拡張
   - 条件付き通知
   - スケジュール管理
   - メトリクス収集

3. カスタムビューの作成
   - ダッシュボード
   - 進捗レポート
   - チーム別サマリー

## 次のステップ
この演習が完了したら、実際のプロジェクトでの活用方法を検討し、カスタマイズした運用フローを構築しましょう。
