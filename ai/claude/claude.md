# 安装

```shell
npm install -g @anthropic-ai/claude-code

```







```shell
claude mcp add --transport http <name> <url>
```

- `claude mcp add`：向本地配置注册一个新的 MCP 工具服务器。
- `--transport http`：表示该 MCP Server 通过 HTTP 协议通信（另一种是 `stdio`，用于本地进程通信）。
- `<name>`：你给这个 MCP 服务起的本地别名（名称）。
- `<url>`：MCP Server 实际运行的地址和端口。

### 添加了一个名为 `xiaohongshu-mcp` 的 MCP 服务。

```sh
claude mcp add --transport http xiaohongshu-mcp http://localhost:18060/mcp

输出 => Added HTTP MCP server xiaohongshu-mcp with URL: http://localhost:18060/mcp to local config
File modified: C:\Users\DELL\.claude.json [project: E:\myinfomation\helailiang\doc\ai\claude]
```

1. 这是一个全局或用户级的配置文件，用于记录你本地注册的所有 MCP Servers。
2. 🧩 `[project: ...]` 表示当前你在某个项目目录下操作（可能是支持 MCP 的 IDE 插件或 CLI 工具的上下文提示），但它不影响 `.claude.json` 的存储位置。

