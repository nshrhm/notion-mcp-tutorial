# ページ情報の取得

## 概要
このガイドでは、特定のNotionページの情報を取得する方法を説明します。ページIDがわかっていれば、`API-retrieve-a-page` ツールを使って詳細な情報を直接取得できます。

## ページ情報の取得方法

### 1. ページIDを指定して取得 (推奨)
   - 取得したいページのIDが既にわかっている場合、`API-retrieve-a-page` ツールを使うのが最も直接的で効率的です。
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-a-page</tool_name>
<arguments>
{
  "page_id": "取得したいページのID" // 例: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
</arguments>
</use_mcp_tool>
```
   - このツールは、ページのプロパティ、作成日時、最終更新日時、URLなどを含む詳細なページオブジェクトを返します。

### 2. 検索結果からの情報利用
   - [前のガイド](search_pages.md)で説明した `API-post-search` でも、検索結果の一部として基本的なページ情報（ID、タイトル、作成/更新日時など）が含まれています。
   - ページIDが不明な場合にまず検索し、その結果からIDを取得して `API-retrieve-a-page` を呼び出す、という流れが一般的です。
   - 検索結果だけで十分な場合は、そのまま利用することも可能です。

```typescript
// 検索結果 (results) からIDを取得して詳細情報を取得する例
const pageId = results[0].id; // resultsはAPI-post-searchの結果配列

<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-retrieve-a-page</tool_name>
<arguments>
{
  "page_id": pageId
}
</arguments>
</use_mcp_tool>
```

## 情報の活用方法

### 1. 取得したページ情報の表示
   - `API-retrieve-a-page` の結果 (pageオブジェクト) を使って情報を表示します。
```javascript
function displayPageInfo(page) { // page は API-retrieve-a-page の結果
  // タイトルプロパティが存在し、内容があるか確認
  const titleProperty = page.properties.title; // プロパティ名は異なる場合があるため要確認
  const titleText = titleProperty?.title?.[0]?.text?.content || 'タイトルなし';

  console.log(`
    ID: ${page.id}
    タイトル: ${titleText}
    作成日時: ${new Date(page.created_time).toLocaleString()}
    最終更新: ${new Date(page.last_edited_time).toLocaleString()}
    URL: ${page.url}
    アーカイブ済: ${page.archived}
  `);
  // 必要に応じて他のプロパティも表示
  // console.log('プロパティ:', page.properties);
}
```
   **注意:** ページのタイトルプロパティの名前は 'title' とは限らない場合があります（特にデータベース内のページ）。実際のプロパティ名に合わせてコードを調整してください。

### 2. コメントでの情報追加
   - 取得したページIDを使って、そのページにコメントを追加できます。
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
- 指定した `page_id` が存在しない (Not Found)
- ページへのアクセス権限がない (Unauthorized/Forbidden)
- リクエストが無効 (Invalid Request)
- ネットワークエラーやサーバーエラー

### 2. エラー処理例 (`API-retrieve-a-page` 呼び出し)
```javascript
async function getPageDetails(pageId) {
  try {
    // MCPツールを呼び出す (実際の呼び出し方は環境による)
    const pageData = await callMcpTool('notionApi', 'API-retrieve-a-page', { page_id: pageId });
    // 成功した場合、ページデータを処理
    displayPageInfo(pageData); // 上記の表示関数を呼び出す例
    return pageData;
  } catch (error) {
    // MCPツールからのエラー応答を想定
    console.error(`ページ情報(ID: ${pageId})の取得に失敗しました。`);
    if (error.response?.data?.code === 'object_not_found') {
      console.error("エラー: 指定されたページが見つかりません。");
    } else if (error.response?.status === 401 || error.response?.status === 403) {
      console.error("エラー: ページへのアクセス権限がありません。インテグレーションの設定を確認してください。");
    } else {
      console.error("エラー詳細:", error.message || error);
    }
    return null;
  }
}

// --- callMcpTool 関数の仮実装 (実際の実装に合わせてください) ---
async function callMcpTool(serverName, toolName, args) {
  // ここで実際にMCPツールを呼び出す処理を実装します。
  // 例: fetch API やライブラリを使ってMCPクライアントと通信するなど。
  // この例では成功した結果を返すダミー実装とします。
  console.log(`Calling MCP Tool: ${serverName}.${toolName} with args:`, args);
  // --- 実際のAPI呼び出し ---
  // const response = await fetch(...);
  // if (!response.ok) throw new Error('API call failed');
  // return await response.json();

  // ダミーデータ (実際はAPIからの応答)
  if (args.page_id === "valid-page-id") {
    return {
      object: 'page',
      id: args.page_id,
      created_time: '2024-01-01T00:00:00.000Z',
      last_edited_time: '2024-01-10T12:00:00.000Z',
      archived: false,
      properties: { /* ... ページのプロパティ ... */
        'title': { // プロパティ名は実際のものに合わせる
          id: 'title',
          type: 'title',
          title: [ { type: 'text', text: { content: 'サンプルページ', link: null }, annotations: {}, plain_text: 'サンプルページ', href: null } ]
        }
      },
      url: 'https://www.notion.so/valid-page-id'
      // ... 他のページ情報
    };
  } else {
     // エラーをシミュレート
     const error = new Error("Simulated API Error");
     error.response = { status: 404, data: { code: 'object_not_found', message: 'Could not find page with ID: ' + args.page_id } };
     throw error;
  }
}

// 関数の呼び出し例
getPageDetails("valid-page-id");
getPageDetails("invalid-page-id");

```
   **注意:** 上記の `callMcpTool` 関数は仮の実装です。実際のMCPクライアントの呼び出し方に合わせて実装してください。

## ベストプラクティス

1.  **IDの正確性:** 正しいページIDを使用していることを確認してください。
2.  **権限の確認:** インテグレーションが対象ページにアクセスできる権限を持っているか事前に確認しましょう。
3.  **必要な情報のみ取得:** `API-retrieve-a-page` は詳細な情報を返しますが、もし特定のプロパティだけが必要な場合は、`filter_properties` パラメータ（ツールスキーマ参照）を使って応答サイズを削減することを検討してください。
4.  **キャッシュ:** 頻繁にアクセスするページの情報をキャッシュすることで、API呼び出し回数を減らし、パフォーマンスを向上させることができます（ただし、情報の鮮度に注意が必要です）。

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
