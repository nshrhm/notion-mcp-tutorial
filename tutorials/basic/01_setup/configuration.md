# Notion MCP 設定ガイド

## 概要
このガイドでは、Notion MCPの詳細な設定方法と、API制限に対応するための推奨設定について説明します。

## API設定

### 1. 基本設定
```json
{
  "notion": {
    "auth": "YOUR_TOKEN",
    "version": "2022-06-28",
    "debug": true,  // デバッグモードを有効化
    "error_handling": {
      "retry": {
        "max_attempts": 3,
        "delay": 1000
      },
      "fallback": {
        "enabled": true,
        "notification": true
      }
    }
  }
}
```

### 2. デバッグモード
```json
{
  "debug": {
    "log_level": "info",     // debug, info, warn, error
    "output": "console",     // console, file
    "file_path": "./logs/notion-mcp.log",
    "include_response": true
  }
}
```

## エラーハンドリング設定

### 1. 基本的なエラー処理
```json
{
  "error_handling": {
    "default": {
      "retry": true,
      "max_attempts": 3,
      "notify": true
    },
    "api_limitation": {
      "notify": true,
      "fallback": "ui_operation",
      "message_template": "API制限により操作できません。UIでの操作をお願いします。"
    }
  }
}
```

### 2. カスタムエラーメッセージ
```json
{
  "error_messages": {
    "database_creation": {
      "error": "データベースの作成はUIから行ってください。",
      "solution": "NotionのUI上でデータベースを作成し、IDを取得してください。"
    },
    "page_creation": {
      "error": "ページの作成はUIから行ってください。",
      "solution": "NotionのUI上でページを作成し、IDを取得してください。"
    }
  }
}
```

## 利用可能なAPIメソッド

### 1. 検索系
```json
{
  "available_methods": {
    "search": {
      "API-post-search": {
        "enabled": true,
        "rate_limit": 100
      }
    }
  }
}
```

### 2. コメント系
```json
{
  "available_methods": {
    "comments": {
      "API-create-a-comment": {
        "enabled": true,
        "rate_limit": 100
      }
    }
  }
}
```

### 3. 情報取得系
```json
{
  "available_methods": {
    "get": {
      "API-get-self": {
        "enabled": true,
        "rate_limit": 100
      }
    }
  }
}
```

## 権限設定

### 1. 必要な権限
```json
{
  "capabilities": {
    "content": {
      "read": true,
      "insert": true,
      "update": true
    },
    "comments": {
      "read": true,
      "create": true
    }
  }
}
```

### 2. アクセス制御
```json
{
  "access_control": {
    "workspace": {
      "read": true,
      "comment": true
    },
    "page": {
      "read": true,
      "comment": true
    }
  }
}
```

## デバッグとログ

### 1. ログ設定
```json
{
  "logging": {
    "enabled": true,
    "level": "info",
    "format": "json",
    "outputs": [
      {
        "type": "console",
        "colored": true
      },
      {
        "type": "file",
        "path": "./logs/notion-mcp.log",
        "rotation": {
          "size": "10MB",
          "keep": 5
        }
      }
    ]
  }
}
```

### 2. デバッグ情報
```json
{
  "debug_info": {
    "api_calls": true,
    "response_time": true,
    "error_details": true
  }
}
```

## 使用例

### 1. 基本設定の適用
```typescript
// config/notion-mcp.json
{
  "notion": {
    "auth": "YOUR_TOKEN",
    "version": "2022-06-28",
    "debug": true,
    "error_handling": {
      "retry": {
        "enabled": true,
        "max_attempts": 3
      }
    },
    "logging": {
      "enabled": true,
      "level": "info"
    }
  }
}
```

### 2. エラーハンドリングの使用
```typescript
// エラー発生時の処理例
try {
  // API呼び出し
} catch (error) {
  if (error.code === "api_limitation") {
    // UI操作への切り替えを提案
    showFallbackMessage();
  }
}
```

## 推奨設定

### 1. 開発環境
```json
{
  "environment": "development",
  "debug": true,
  "logging": {
    "level": "debug",
    "output": ["console", "file"]
  }
}
```

### 2. 本番環境
```json
{
  "environment": "production",
  "debug": false,
  "logging": {
    "level": "warn",
    "output": ["file"]
  }
}
```

## トラブルシューティング

### よくある問題と対処方法

1. API制限関連
   - エラーメッセージを確認
   - 利用可能なメソッドを使用
   - 代替アプローチを検討

2. 権限エラー
   - 設定を確認
   - トークンの更新
   - アクセス権の見直し

3. 接続エラー
   - ネットワーク確認
   - トークン確認
   - リトライ設定の調整

## 次のステップ
設定が完了したら、[基本操作](../02_basic_operations/search_pages.md)に進んでください。
