# OpenHands を事務作業に活用する検討

OpenHands プロジェクトを、プログラミング以外の一般的な事務ワークフローに転用できるかを評価します。コードベースはモジュール化されたエージェント構成、Model Context Protocol (MCP) によるツール統合、そして柔軟なマイクロエージェントを備えており、文書管理やメール送信といった業務にも応用可能です。

## 関連アーキテクチャ

OpenHands はエージェント、イベントストリーム、ランタイム、サーバーコンポーネントで構成されています。主要クラスと制御フローは `openhands/README.md` に概説されています。

```
(README.md の LLM、Agent、AgentController、Runtime、イベントループの説明からの抜粋)
```

これらのコンポーネントはソフトウェア開発以外のワークフローにも十分汎用的です。

## MCP 連携

MCP を利用すると、OpenHands から SSE や HTTP を経由して外部ツールを呼び出せます。クライアント実装は `openhands/mcp/client.py` にあります。

```
(MCPClient クラスと connect_http の抜粋)
```

ランタイムでは MCPProxyManager が MCP サーバーをマウントします。

```
(runtime/mcp/proxy/manager.py の MCPProxyManager 抜粋)
```

MCP サーバーは `core/config/mcp_config.py` で設定します。

```
(サーバークラスとバリデーション部分の抜粋)
```

設定方法の詳細は `docs/usage/mcp.mdx` に解説されています。

```
(設定例を含む抜粋)
```

`send_email` などの機能を提供する MCP サーバーを定義すれば、コアロジックを変更せずにメール送信を実現できます。

## マイクロエージェント

マイクロエージェントはキーワードやリポジトリ設定に応じて自動的に読み込まれる、ドメイン特化の指示を提供します。詳細は `microagents/README.md` を参照してください。

```
(マイクロエージェントの種類を説明する抜粋)
```

これを活用すれば、スケジューリングやレポート作成など、事務作業に適したガイドをエージェントに与えられます。リポジトリ固有のマイクロエージェントを作成することで、各プロジェクトに合わせたカスタマイズも可能です。

## 検討事項

1. **ランタイム** – 現状のランタイムは Git やシェルコマンドなど開発向け機能を中心としています。事務作業ではコード編集機能を無効化し、Office 操作などを MCP ツールで補うことが考えられます。
2. **セキュリティ** – メール送信や機密文書の扱いでは権限管理や監査ログがより重要になります。OpenHands の権限設定を確認した上で利用してください。
3. **ユーザー体験** – デフォルトのプロンプトや UI は開発者向けです。マイクロエージェントや UI 文言を編集し、事務スタッフでも使いやすいよう調整すると良いでしょう。

## 結論

OpenHands は十分にモジュール化されており、非プログラミングの業務にも転用可能です。メール送信などの機能を備えた MCP サーバーを用意し、適切なマイクロエージェントを記述することで、既存のエージェント・ランタイム・サーバー構成を活かして一般的な事務ワークフローを自動化できます。
