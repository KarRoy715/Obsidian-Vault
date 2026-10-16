---
onenote-id: 76c843f0-9f99-4515-a062-2e9f8d6b1d8e:1
---
**MCP****：可以使****AI****拥有使用外部软件的能力**

- **联网能力：****barve-search**
- 命令行输入：claude mcp add brave-search -e BRAVE_API_KEY=这里填你的密钥 --npx -y @brave/brave-search-mcp-server
- **网页读取：****fetch**
- claude mcp add fetch --npx -y @modelcontextprotocol/server-fetch
- **图片读取：****ScreenshotMCP**
- claude mcp add screenshot --npx -y mcp-screenshot-service

**多****agent**

|  | Subagents | Agent Teams |
| --- | --- | --- |
| 通信 | 子agent结果返回主agent，主agent决定接下来调度哪个子agent；各个agent是分开独立的 | 各个子agent之间是可以自己进行交互通信的 |
| 上下文 | 共享主对话（1个窗口） | 每人独立窗口（N个窗口），共享上下文信息 |
| 成本 | 低 | 高（N倍） |
| 擅长 | 出量、标准化、可预期 | 有纠错、逻辑判断、角度挖掘 |

**Agent team****默认关闭**

![[Cloude code image.png]]

**Agent team** **可能不能兼容其他模型**