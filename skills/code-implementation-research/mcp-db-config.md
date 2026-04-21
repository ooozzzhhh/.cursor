# 测试库连接与 Agent 用法（MySQL MCP）

本文件供 **Agent 做代码+库表联调调研** 时使用。**产品交付文档**（PRD、问题分析报告、用例等）中 **只写「基于哪个库 / 哪个环境」**，不要写 MCP 工具名、`mcp.json` 路径等技术细节。

---

## 1. 配置从哪里来

- **Cursor MCP 声明**：优先看本仓库工作区内的 `.cursor/mcp.json`；若使用用户级配置，则为 macOS/Linux 下的 `~/.cursor/mcp.json`（Windows 为 `%USERPROFILE%\.cursor\mcp.json`）。其中 `scp-test` / `scp-prod` 等条目里的 **账号口令以你本机该文件为准**，勿写入本仓库。
- **Cursor 里映射的服务名**：工具侧可能显示为 `user-scp-test`、`user-scp-prod` 等，与 `mcp.json` 里的 key 对应，以当前会话可用的 MCP 服务器名为准。

---

## 2. 连接时必填 `database`

调用 `connect_db` 时 **必须** 传入 `database`（库名），仅 `host/user/password` 往往不够。

**本仓库约定（SDS 与主数据交叉核对）**：

| 用途 | 默认库名 | 说明 |
|------|----------|------|
| 需求优先级、客户、库存点物品基础信息、销售片区等 **与 SDS 功能相关的表** | **`scp_mps`** | 表结构核对与问题分析默认采用此库；若任务指定其他库名则以任务为准。 |

主机、端口、用户与 `mcp.json` 中 `scp-test`（或当前环境）保持一致。

---

## 3. Agent 推荐步骤（内部）

1. 从 `mcp.json` 读取 `host`、`port`、`user`、`password`（或由用户已在环境中配置好的等价参数）。
2. `connect_db`，**带上 `database`**（默认 `scp_mps`，除非任务指定其他库）。
3. 使用 `describe_table`、`query`（仅 `SELECT` / `information_schema`）核对表结构；**禁止**在未获用户明确授权时对生产库执行变更类工具。

---

## 4. `mcp.json` 条目形态（示例，口令请用本机真实值）

以下为结构示意，**勿将真实口令提交到 Git**：

```json
{
  "mcpServers": {
    "scp-test": {
      "command": "/usr/local/bin/npx",
      "args": [
        "-y",
        "mcp-mysql-server",
        "--host=<主机>",
        "--port=3306",
        "--user=<用户>",
        "--password=<口令>"
      ]
    }
  }
}
```

> **说明**：`npx` 路径以本机为准；Intel + Homebrew 常见为 `/usr/local/bin/npx`，Apple Silicon 常见为 `/opt/homebrew/bin/npx`。终端执行 `which npx` 可核对。

---

## 5. 与产品文档的分工

| 载体 | 写法 |
|------|------|
| 产品问题分析 / PRD / 知识库 | **只写**：例如「测试库：`scp_mps`，`10.7.60.157:3306`」及表名、字段结论。 |
| 本文件 | Agent 连接步骤、默认库约定、`mcp.json` 位置；**不写进**对外产品报告正文。 |

若 MCP 不可用，须在调研结论中说明「未能连库」，可退化为 Mapper/实体推断，并标明不确定性。
