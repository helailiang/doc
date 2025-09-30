```json
{
  "mcpServers": { // 定义一个名为 "mcpServers" 的对象，包含多个服务器配置
  
    "github.com/modelcontextprotocol/servers/tree/main/src/github": { // 定义一个服务器配置，键名为服务器的标识
      "command": "cmd", // 指定要运行的命令行程序，这里是 Windows 的 cmd
      "args": [ // 定义传递给命令行程序的参数
        "/c", // 命令行参数，表示执行指定的命令并终止
        "npx", // 使用 npx 运行 npm 包
        "-y", // 自动回答所有提示为 "yes"
        "@modelcontextprotocol/server-github" // 要运行的 npm 包名称
      ],
      "env": { // 定义环境变量
        "GITHUB_PERSONAL_ACCESS_TOKEN": "Your token" // GitHub 个人访问令牌，用于身份验证
      },
      "disabled": false, // 指定该服务器配置是否被禁用，false 表示启用
      "autoApprove": [], // 自动批准的操作列表，这里为空
      "timeout": 30 // 指定命令的超时时间（秒）
    },

    "filesystem": { // 定义另一个服务器配置，键名为 "filesystem"
      "command": "cmd", // 同样使用 Windows 的 cmd
      "args": [ // 定义传递给命令行程序的参数
        "/c", // 命令行参数，表示执行指定的命令并终止
        "npx", // 使用 npx 运行 npm 包
        "-y", // 自动回答所有提示为 "yes"
        "@modelcontextprotocol/server-filesystem", // 要运行的 npm 包名称
        "D:\\Users\\16009\\Desktop" // 传递给 npm 包的参数，这里是一个文件路径
      ],
      "disabled": false, // 指定该服务器配置是否被禁用，false 表示启用
      "autoApprove": [] // 自动批准的操作列表，这里为空
    }
  }
}

```



























问题：

·1. 400 This model's maximum context length is 65536 tokens. However, you requested 210780 tokens (210780 in the messages, 0 in the completion). Please reduce the length of the messages or completion.



