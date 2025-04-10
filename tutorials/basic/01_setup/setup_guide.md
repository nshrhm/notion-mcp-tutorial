# Notion MCP セットアップガイド

## はじめに
このガイドでは、Notion MCPの初期設定と基本的な使用方法について説明します。

## 重要な注意事項
⚠️ API制限について
- 一部のAPI機能（データベース作成、ページ作成など）に制限があります
- UIとAPIを組み合わせたハイブリッドアプローチを推奨します
- 詳細は[API制限事項](api_limitations.md)を参照してください

## セットアップ手順

### 1. NotionのUI設定
1. ワークスペースの準備
   - 新規ワークスペースの作成
   - 必要なページやデータベースの作成
   - テンプレートの準備

2. インテグレーションの作成
   - [My Integrations](https://www.notion.so/my-integrations)にアクセス
   - 「新しいインテグレーション」をクリック
   - 名前と対象ワークスペースを設定

3. 権限の設定
   - インテグレーションの基本設定を確認
   - 必要な権限を有効化
   ```
   Required Capabilities:
   - Content Capabilities
     ✓ Read content
     ✓ Update content
     ✓ Insert content
   - Comment Capabilities
     ✓ Read comments
     ✓ Create comments
   ```

### 2. APIトークンの取得
1. インテグレーション設定ページで「シークレットの表示」をクリック
2. トークンをコピー
3. 安全な場所に保管

### 3. ページアクセスの設定
1. 対象ページを開く
2. 「...」メニューから「接続を追加」を選択
3. 作成したインテグレーションを選択
4. アクセス権限を確認

### 4. MCPの設定
1. 設定ファイルの作成
```json
{
  "notionApi": {
    "command": "npx",
    "args": ["-y", "@notionhq/notion-mcp-server"],
    "env": {
      "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer YOUR_TOKEN\", \"Notion-Version\": \"2022-06-28\" }"
    }
  }
}
```

2. 動作確認
```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-get-self</tool_name>
<arguments>
{}
</arguments>
</use_mcp_tool>
```

## 推奨される使用方法

### 1. ハイブリッドアプローチ
1. NotionのUIを使用
   - データベースの作成と構造設定
   - テンプレートページの準備
   - 初期データの設定

2. APIを使用
   - データの検索と取得
   - コメントによる情報追加
   - 自動更新の実装

### 2. エラー処理の基本
1. API制限の確認
   - 利用可能なメソッドの確認
   - 権限設定の確認
   - エラーメッセージの理解

2. 代替フローの準備
   - UI操作への切り替え
   - 手動操作の手順提供
   - ユーザーへの通知

## トラブルシューティング

### よくある問題と解決方法

1. アクセス権限エラー
   - インテグレーションの権限を確認
   - ページへのアクセス権限を確認
   - トークンの有効性を確認

2. API制限関連
   - 利用可能なメソッドを確認
   - 代替アプローチを検討
   - ドキュメントを参照

3. 接続エラー
   - トークンの正確性を確認
   - ネットワーク接続を確認
   - APIのバージョンを確認

## 次のステップ
基本設定が完了したら、以下の手順で操作方法を学習してください：

1. [設定の詳細](configuration.md)
2. [基本的な操作方法](../02_basic_operations/search_pages.md)
3. [演習問題](../03_exercises/exercise_1.md)

## サポート
- 技術的な質問: APIドキュメントを参照
- バグ報告: GitHubイシューで報告
- 一般的な質問: コミュニティフォーラムを利用
