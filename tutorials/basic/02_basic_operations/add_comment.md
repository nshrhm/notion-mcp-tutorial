# コメントの追加

## 概要
このガイドでは、Notionページにコメントを追加する方法を説明します。コメント機能は、利用可能なAPIの重要な機能の1つで、ページへの情報追加や更新通知に活用できます。

## コメントの基本

### 1. シンプルなコメント
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "ページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "基本的なコメントです。"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 2. リッチテキストコメント
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "ページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "重要な通知\n\n"
      },
      "annotations": {
        "bold": true,
        "color": "red"
      }
    },
    {
      "text": {
        "content": "このコメントには書式が含まれています。"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## コメントの活用方法

### 1. 更新通知
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "ページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "【更新通知】\n\n"
      },
      "annotations": {
        "bold": true
      }
    },
    {
      "text": {
        "content": "更新内容:\n- データの更新\n- ステータスの変更\n- 期限の調整\n\n"
      }
    },
    {
      "text": {
        "content": "更新日時: "
      }
    },
    {
      "text": {
        "content": new Date().toLocaleString()
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 2. 進捗報告
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "ページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "進捗状況報告\n\n"
      },
      "annotations": {
        "bold": true,
        "underline": true
      }
    },
    {
      "text": {
        "content": "現在の進捗: 50%\n\n完了したタスク:\n- 要件定義\n- 基本設計\n\n次のアクション:\n1. 詳細設計の開始\n2. レビュー実施"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## コメントの書式設定

### 1. テキストの装飾
- bold: 太字
- italic: 斜体
- strikethrough: 取り消し線
- underline: 下線
- code: コード形式
- color: テキストカラー

### 2. 構造化されたコメント
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "ページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "セクション1: 概要\n\n"
      },
      "annotations": {
        "bold": true,
        "color": "blue"
      }
    },
    {
      "text": {
        "content": "主な内容の説明\n\n"
      }
    },
    {
      "text": {
        "content": "セクション2: 詳細\n\n"
      },
      "annotations": {
        "bold": true,
        "color": "blue"
      }
    },
    {
      "text": {
        "content": "詳細な説明と補足情報"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## エラーハンドリング

### 1. 一般的なエラー
- 権限エラー
- テキスト形式エラー
- ページIDエラー

### 2. エラー処理例
```javascript
async function addComment(pageId, content) {
  try {
    // コメント追加のAPI呼び出し
  } catch (error) {
    if (error.code === "unauthorized") {
      console.error("コメント追加の権限がありません");
    } else if (error.code === "invalid_request") {
      console.error("リクエスト形式が不正です");
    }
    throw error;
  }
}
```

## ベストプラクティス

### 1. コメント構造
- 明確な見出し
- 論理的な構造化
- 適切な書式設定

### 2. 情報の整理
- 重要度に応じた強調
- 日時情報の明記
- アクション項目の明確化

### 3. 運用のヒント
- 定期的な更新報告
- 重要な変更の通知
- フィードバックの収集

## よくある質問

### Q: コメントが追加できない
1. ページへのアクセス権確認
2. インテグレーションの権限確認
3. リクエスト形式の確認

### Q: 書式が反映されない
1. アノテーション設定の確認
2. リッチテキスト形式の確認
3. サポートされている書式の確認

## 次のステップ
基本操作を理解したら、[演習問題](../03_exercises/exercise_1.md)に進んで実践的な使用方法を学びましょう。
