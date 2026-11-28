# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：`search_materials`（`app.agent`）、`glossary_search_glossary`（`app.mcp_agent`）
- STEP 2 の [tool call]：`[tool call]   glossary_search_glossary({'query': '3レベル'})`
- STEP 3 の [tool call]：`[tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})`
- STEP 4 の [tool call]：`[tool call]   aws_aws___search_documentation({'topics': ['general'], 'search_phrase': 'Amazon S3 versioning overview'})`
- 気づき・疑問：エージェント本体のコードを変更しなくても、`app/mcp_servers.json` にサーバーを追加するだけで、ローカル起動（`command`）や外部接続（`url`）のツールがモデルから同じ形式で使えるようになる仕組みが分かりやすかった。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：エージェント側（`app/mcp_servers.json`、`app/mcp_agent.py`）は変更せず、サーバー側の `app/mcp_server.py` に `server.tool()(tools.get_lecture_summary)` の1行だけを書き足してツールを公開した。その後、`git status` で変更箇所を確認し、`--list-tools`、`--trace` 付きでの質問、`uv run pytest -q` を実行した。
- 結果：
  - `git status`：変更ファイルは `app/mcp_server.py` のみであることを確認した。
  - `--list-tools`：モデルに見えているツールが7個から8個に増え、`glossary_get_lecture_summary` が追加された。
  - `--trace`（「第2回では何を学びますか？」）：`[tool call]   glossary_get_lecture_summary({'lecture': 2})` が呼ばれ、第2回（ファインチューニングとLoRA）の内容が回答された。
  - `uv run pytest -q`：`21 passed in 4.31s` で全テストが通った。
- 気づき・疑問：エージェントは起動するたびに MCP サーバーへ公開ツールの一覧を問い合わせるため、サーバー側を1行修正するだけでエージェント側を一切変えずに新しいツールが使えるようになるメリットを実感できた。
