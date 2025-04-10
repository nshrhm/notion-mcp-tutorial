# ページ情報の取得

## 概要
このガイドでは、検索で見つけたページの情報を取得する方法を説明します。現在のAPI制限により、ページの検索と情報取得が主な操作方法となります。

## ページ情報の取得方法

### 1. 検索からの取得
```typescript
// まず、ページを検索
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{
  "query": "プロジェクト",
  "filter": {
    "property": "object",
    "value": "page"
  }
}
</arguments>
</use_mcp_tool>

// 検索結果から必要な情報を抽出
const pageInfo = {
  id: "検索結果のページID",
  title: "検索結果のタイトル",
  url: "ページのURL"
};
```

### 2. プロパティの取得
```typescript
// 検索結果からプロパティを取得
const properties = {
  title: results[0].properties.title,
  created_time: results[0].created_time,
  last_edited_time: results[0].last_edited_time
};
```

## 情報の活用方法

### 1. 基本情報の表示
```javascript
function displayPageInfo(page) {
  console.log(`
    タイトル: ${page.properties.title.title[0].text.content}
    作成日時: ${new Date(page.created_time).toLocaleString()}
    最終更新: ${new Date(page.last_edited_time).toLocaleString()}
    URL: ${page.url}
  `);
}
```

### 2. コメントでの情報追加
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
        "content": "ページ情報を確認しました。\n\n"
      }
    },
    {
      "text": {
        "content": "最終更新: " + new Date().toLocaleString()
      }
    }
  ]
}
</arguments>
</use_mcp_tool>
```

## エラーハンドリング

### 1. 一般的なエラー
- ページが存在しない
- アクセス権限がない
- ネットワークエラー

### 2. エラー処理例
```javascript
async function getPageInfo(pageId) {
  try {
    const results = await searchPage(pageId);
    if (results.length === 0) {
      throw new Error("ページが見つかりません");
    }
    return results[0];
  } catch (error) {
    console.error("ページ情報の取得に失敗:", error);
    return null;
  }
}
```

## 代替アプローチ

### 1. UIとの併用
1. NotionのUIでページを開く
2. 必要な情報を確認
3. APIで検索して参照

### 2. 情報の保存
- 重要なページIDはローカルに保存
- 頻繁にアクセスする情報をキャッシュ
- 定期的な情報更新の自動化

## ベストプラクティス

### 1. 効率的な情報取得
- 必要な情報のみを取得
- 検索結果のキャッシュ
- バッチ処理の活用

### 2. エラー対策
- タイムアウトの設定
- リトライメカニズムの実装
- エラーログの記録

### 3. パフォーマンス最適化
- 不要な検索の削減
- 結果のキャッシュ
- 定期的な更新スケジュール

## よくある質問

### Q: ページが見つからない
1. 検索クエリの確認
2. アクセス権限の確認
3. ページの存在確認

### Q: 情報が古い
1. キャッシュのクリア
2. 強制再取得の実行
3. 更新タイミングの調整

## 次のステップ
ページ情報の取得を理解したら、[コメントの追加](add_comment.md)に進んでください。
