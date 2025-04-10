# 演習1の解答例と解説

## 課題1: ページの検索

### 解答例
```typescript
// 1. プロジェクトの検索
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "sort": {
    "direction": "descending",
    "timestamp": "last_edited_time"
  },
  "page_size": 3
}
</arguments>
</use_mcp_tool>
```

### 解説
- `query`: 検索キーワードを指定
- `sort`: 最新の更新順に並び替え
- `page_size`: 結果を3件に制限
- `direction`: "descending"で降順（最新順）を指定

## 課題2: ページの取得と確認

### 解答例
```typescript
// 各ページの詳細情報取得
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-a-page</tool_name>
<arguments>
{
  "page_id": "検索で取得したページID"
  // filter_properties は特定のプロパティIDを文字列で指定しますが、
  // 基本演習では省略します。全プロパティが取得されます。
}
</arguments>
</use_mcp_tool>
```

### 解説
- `page_id`: 検索結果から取得したIDを使用
- `filter_properties`: 必要なプロパティのみを指定
- 結果は各ページの基本情報を含む

## 課題3: コメントの追加

### 解答例
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-create-a-comment</tool_name>
<arguments>
{
  "parent": {
    "page_id": "選択したページID"
  },
  "rich_text": [
    {
      "text": {
        "content": "【進捗確認報告】\n"
      },
      "annotations": {
        "bold": true
      }
    },
    {
      "text": {
        "content": "確認日時: 2025/04/09 15:00\n\n"
      }
    },
    {
      "text": {
        "content": "進捗状況:\n"
      },
      "annotations": {
        "underline": true
      }
    },
    {
      "text": {
        "content": "・計画フェーズ完了（50%）\n・設計フェーズ開始\n\n"
      }
    },
    {
      "text": {
        "content": "次のアクション:\n"
      },
      "annotations": {
        "underline": true
      }
    },
    {
      "text": {
        "content": "1. タスクAの完了（期限: 4/15）\n2. レビュー実施（4/16予定）\n3. 文書更新（4/17まで）"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

### 解説
1. リッチテキストの構造化
   - 見出し的な部分は太字や下線で強調
   - 日時情報は明確に記載
   - 箇条書きで見やすく整形

2. コメントの構成
   - ヘッダー: 目的を明確に
   - 日時情報: 実施時刻を記録
   - 進捗状況: 現在の状態を数値で
   - アクション項目: 具体的なタスクと期限

3. フォーマットの工夫
   - 空行で視認性を向上
   - 記号を使用して構造化
   - 期限を明確に記載

## 発展的な解答例

### 1. 動的なコメント生成
タスクの状態に応じて異なるコメントを生成する例：

```typescript
// ステータスに基づくコメント
const status = "遅延"; // または "順調" など
const commentContent = status === "遅延" ? 
  "対策が必要です。\n1. 原因分析\n2. リカバリープラン作成" :
  "予定通り進行中です。\n次のマイルストーンに向けて継続";

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
        "content": `状態: ${status}\n\n${commentContent}`
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## ベストプラクティス
1. 検索時の考慮点
   - 適切な検索範囲の設定
   - 結果数の制限による効率化
   - ソート条件の明確な指定

2. ページ情報取得時の工夫
   - 必要な情報のみを取得
   - プロパティの型に注意
   - エラーハンドリングの実装

3. コメント作成のコツ
   - 構造化された情報提示
   - 視認性の高いフォーマット
   - アクションアイテムの明確化
