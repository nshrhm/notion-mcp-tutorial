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
   - 以下のJSONを参考に、MCP設定ファイル（例: `cline_mcp_settings.json`）に `notionApi` の設定を追加します。
   - **重要:** `"YOUR_TOKEN"` の部分は、ステップ2で取得した実際のAPIトークン（シークレット）に置き換えてください。

```json
{
  "mcpServers": { // 他のサーバー設定がある場合は、この中に追記します
    "notionApi": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        // 注意: この環境変数名はNotion MCPサーバーの仕様に基づいています。
        // トークンとAPIバージョンをヘッダーとして設定します。
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer YOUR_TOKEN\", \"Notion-Version\": \"2022-06-28\" }"
      },
      "disabled": false, // サーバーを有効にする
      "autoApprove": [] // 自動承認は設定しない
    }
  }
}
```
   - 設定ファイルを保存すると、MCPサーバーが自動的に起動します。

2. 動作確認
   - 設定が正しく行われ、Notion APIへの認証が成功したかを確認するために、以下のツールを実行します。
   - このコマンドは、あなたのNotionアカウント（ボットユーザー）自身の情報を取得します。

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
   - 前述の通り、Notion APIには一部制限があるため、UI操作とAPI操作を組み合わせる**ハイブリッドアプローチ**が現実的かつ効果的です。
   - これにより、UIの柔軟な操作性（データベース構造の設計など）と、APIによる自動化（定型的なデータ取得や更新など）の利点を両立できます。

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

---

**## よくある質問 (FAQ)**

**Q: このチュートリアルはNotion APIがメインで、MCPは使っていないのですか？**

**A:** 確かに、チュートリアルのコード例はNotion APIの各機能（ページの検索、作成、更新など）の使い方を示しているため、Notion APIが中心に見えるかもしれません。

しかし、重要な点として、このチュートリアルではそれらのNotion API機能を **MCP (Model Context Protocol) を介して利用しています**。

コード例に出てくる `<use_mcp_tool>` タグが、MCPの仕組みを使っている証拠です。

```typescript
<use_mcp_tool>
<server_name>notionApi</server_name>
<tool_name>API-post-search</tool_name>
<arguments>
{ ... }
</arguments>
</use_mcp_tool>
```

これは、直接Notion APIを呼び出すのではなく、`notionApi` という名前の **MCPサーバー** に対して、`API-post-search` という名前の **ツール（Notion APIの検索機能に対応）** の実行をリクエストしています。MCPサーバーが、私たちのリクエストを受け取り、認証情報（APIトークンなど）を付与して、実際にNotion APIと通信する役割を担っています。

MCPを使うことで、
*   様々なAPI（将来的にはNotion以外も）を統一的な方法（`<use_mcp_tool>`）で呼び出せる。
*   APIごとの認証情報の管理をMCPサーバーに任せられる。
といったメリットがあります。

このチュートリアルは、「Notion APIの機能をMCP経由でどのように利用するか」を学ぶことを目的としています。
