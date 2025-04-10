# 演習1: 検索とコメント

## 目的
この演習では、Notionページの検索とコメント追加の基本操作を実践します。

## 課題内容

### 課題1: ページの検索
1. Notionワークスペース内で "プロジェクト" という単語を含むページを検索してください
2. 検索結果を最新の更新順に並び替えて表示してください
3. 検索結果から最初の3件のページIDを取得してください

#### 解答例
```typescript
// 1. 基本検索
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

### 課題2: ページの取得と確認
1. 課題1で取得したページIDを使用して、各ページの詳細情報を取得してください
2. 各ページのタイトルとプロパティを確認してください

#### 解答例
```typescript
// ページ情報の取得
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-a-page</tool_name>
<arguments>
{
  "page_id": "検索で取得したページID"
}
</arguments>
</use_mcp_tool>
```

### 課題3: コメントの追加
1. 取得したページの中から1つを選び、進捗状況に関するコメントを追加してください
2. コメントには以下の情報を含めてください：
   - 確認日時
   - 進捗状況
   - 次のアクション

#### 解答例
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
        "content": "進捗確認 (2025/04/09)\n\n"
      }
    },
    {
      "text": {
        "content": "現在の進捗: 50%完了\n"
      }
    },
    {
      "text": {
        "content": "次のアクション:\n- タスクAの完了\n- レビュー実施\n- 文書更新"
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## 評価ポイント
1. 検索機能の適切な使用
   - 検索クエリの正しい指定
   - ソート順の適切な設定
   - 結果の制限設定

2. ページ情報の取得
   - 正しいページIDの使用
   - 必要な情報の抽出

3. コメント追加
   - リッチテキストの適切な構造化
   - 必要情報の明確な記載
   - 整形された読みやすい形式

## ヒント
- 検索時は`page_size`パラメータで結果数を制限できます
- ページ情報取得時は`filter_properties`で必要な情報のみを取得できます
- コメントは複数のリッチテキストブロックを組み合わせることができます

## 発展課題
1. 検索結果をフィルタリングして、特定の条件に合うページのみを表示
2. ページのプロパティに基づいて、動的にコメント内容を生成
3. 複数ページに一括でコメントを追加

## 次のステップ
この演習が完了したら、[演習2](exercise_2.md)に進んでください。
