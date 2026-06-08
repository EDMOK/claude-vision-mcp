<div align="center">

# Claude Vision MCP

让纯文本 LLM 通过 MCP 协议调用视觉 API 分析图片

[![License: MIT](https://img.shields.io/badge/license-MIT-%23000)](LICENSE)

</div>

---

## 最快捷的方式：让 AI 帮你装

把 `https://github.com/EDMOK/claude-vision-mcp.git` 发给你的 AI 工具，告诉它安装这个 Vision MCP。

AI 可以直接按照下面「Claude Code 安装」一节的步骤自动完成安装。

---

## 手动安装

```bash
git clone https://github.com/EDMOK/claude-vision-mcp.git
cd claude-vision-mcp
```

预编译产物在 `dist/index.js`（单文件 ~721KB，已打包所有依赖），**无需 `npm install`**，Node.js >= 20 即可运行。

---

## Claude Code 安装

### 推荐安放位置

将 MCP Server 复制到本机稳定目录，避免依赖 U 盘、临时目录或源码工作目录：

```text
C:\Users\<User>\.claude\mcp-servers\vision-mcp-server
```

需要复制完整目录，至少包含：

```text
dist/
package.json
node_modules/
```

### 注册 MCP Server

使用 `claude mcp add` 注册：

```bash
claude mcp add \
  --scope local \
  --transport stdio \
  --env VISION_API_KEY="sk-..." \
  --env VISION_BASE_URL="https://api.openai.com/v1" \
  --env VISION_MODEL="gpt-4o" \
  vision \
  -- node "C:\Users\<User>\.claude\mcp-servers\vision-mcp-server\dist\index.js"
```

**注意：**

- `--scope`、`--transport`、`--env` 必须放在 MCP Server 名称 `vision` 之前。
- `--` 后面是实际启动 MCP Server 的命令和参数。
- 包含 API Key 的配置应使用 `local` 或 `user` scope，避免使用 `project` scope 导致密钥进入版本控制。

### 验证

```bash
claude mcp get vision
claude mcp list
```

期望看到 `Status: ✓ Connected`。

注册完成后需要重启 Claude Code 会话，新的 MCP 工具才会生效。

---

## 其他 MCP 客户端配置

在 MCP 配置文件的 `mcpServers` 中添加，**将路径替换为你的实际目录**。

### macOS / Linux

`~/.claude.json`：

```json
"vision": {
  "command": "/path/to/claude-vision-mcp/dist/index.js",
  "env": {
    "VISION_API_KEY": "sk-...",
    "VISION_MODEL": "gpt-4o"
  },
  "type": "stdio"
}
```

### Windows

`%USERPROFILE%\.claude.json`：

```json
"vision": {
  "command": "node",
  "args": ["C:\\path\\to\\claude-vision-mcp\\dist\\index.js"],
  "env": {
    "VISION_API_KEY": "sk-...",
    "VISION_BASE_URL": "https://openai.com/v1",
    "VISION_MODEL": "Qwen/Qwen3.5-27B"
  },
  "type": "stdio"
}
```

配置完成后重启 MCP 客户端即可调用 `vision_analyze` 工具。

---

## 参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `image_type` | string | ✅ | `url` / `file` / `clipboard` / `base64` / `multi` |
| `image_data` | string | ❌ | URL、本地路径或 Base64 字符串 |
| `media_type` | string | ❌ | Base64 时需要，如 `image/png` |
| `images` | array | ❌ | `multi` 模式时传多张图片 |
| `prompt` | string | ❌ | 自定义分析提示词 |
| `max_tokens` | number | ❌ | 最大输出 token 数（默认 2048） |

---

## 环境变量

| 变量 | 必填 | 默认值 | 说明 |
|------|------|--------|------|
| `VISION_API_KEY` | ✅ | — | OpenAI 兼容 API 密钥 |
| `VISION_BASE_URL` | ❌ | `https://api.openai.com/v1` | API 地址 |
| `VISION_MODEL` | ❌ | `gpt-4o` | 视觉模型 |
| `VISION_TOOL_DISABLED` | ❌ | `false` | 设为 `true` 隐藏工具 |

---

## 系统要求

- Node.js >= 20
- 剪贴板功能仅 Windows + PowerShell

---

## 更新日志

### v1.1.0

- **工具 description 全面重写** — 6 部分结构化指引（功能说明、必要性、使用场景、5 种 image_type 决策树、prompt 用法、输出说明），让纯文本模型更精准地调用视觉能力
- **剪贴板能力明确告知模型** — 模型现在知道 `clipboard` 模式可直接读取 Windows 系统剪贴板，不再要求用户手动粘贴图片
- **构建输出改为 CommonJS** — 修复 OpenAI SDK 在 ESM 下动态 `require` 的兼容问题
- **参数 description 精简优化** — 每个参数配场景化指引，降低模型误用率

---

## License

MIT
