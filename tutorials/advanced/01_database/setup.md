# Notionデータベースの作成と設定

## はじめに
このチュートリアルでは、NotionのデータベースをAPIを使って作成・管理する方法を学びます。

## 前提条件
- Notion MCPの基本セットアップが完了していること
- 基本的なAPI操作（ページの作成・取得・更新）を理解していること

## データベースの基本概念

### 1. Notionデータベースとは
- 構造化されたデータを管理するための機能
- スプレッドシートのような表形式で情報を整理
- カレンダー、ギャラリー、カンバンなど様々なビューに対応

### 2. データベースの構成要素
1. プロパティ（列）
   - タイトル（必須）
   - テキスト
   - 数値
   - 選択肢
   - 日付
   - ユーザー
   など

2. エントリー（行）
   - データベース内の各レコード
   - ページとして管理される

3. ビュー
   - テーブル
   - カレンダー
   - リスト
   - タイムライン
   など

## データベースの作成

### 1. 空のデータベースの作成
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
        "content": "プロジェクト管理"
      }
    }
  ],
  "properties": {
    "タイトル": {
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
    "担当者": {
      "people": {}
    },
    "期限": {
      "date": {}
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
    }
  }
}
</arguments>
</use_mcp_tool>
```

### 2. プロパティタイプの説明

1. **title**
   - データベースの必須プロパティ
   - 各エントリーのメインタイトル

2. **rich_text**
   - 複数行のテキスト
   - 書式設定可能

3. **number**
   - 数値データ
   - フォーマット指定可能（整数、小数、金額など）

4. **select**
   - 単一選択
   - 事前定義されたオプションから選択

5. **multi_select**
   - 複数選択
   - タグのような使用方法

6. **date**
   - 日付、または期間
   - タイムゾーン設定可能

7. **people**
   - ユーザー参照
   - 複数のユーザーを設定可能

8. **files**
   - ファイルや画像の添付
   - 外部URLの参照も可能

### 3. プロパティの追加と更新
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-update-database</tool_name>
<arguments>
{
  "database_id": "データベースID",
  "properties": {
    "進捗率": {
      "number": {
        "format": "percent"
      }
    },
    "備考": {
      "rich_text": {}
    }
  }
}
</arguments>
</use_mcp_tool>
```

## エラーハンドリング

### よくあるエラーと対処方法

1. 権限エラー
   - データベース作成権限の確認
   - 親ページへのアクセス権限の確認

2. プロパティ設定エラー
   - 必須プロパティ（title）の確認
   - プロパティ名の重複確認
   - 値の形式確認

3. データベースIDエラー
   - IDの形式確認
   - データベースの存在確認

## ベストプラクティス

1. データベース設計
   - 必要最小限のプロパティから開始
   - 段階的な拡張を計画
   - 命名規則の統一

2. プロパティ設定
   - 明確な目的を持ったプロパティ定義
   - 適切なデータ型の選択
   - 選択肢の適切な設定

3. 更新管理
   - 変更履歴の記録
   - バックアップ計画
   - テスト環境での検証

## 次のステップ
データベースの基本設定が完了したら、[クエリとフィルタリング](../02_queries/basic_queries.md)に進んで、データの操作方法を学びましょう。
