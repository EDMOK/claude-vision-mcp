<div align="center">

# Claude Vision MCP

让纯文本 LLM 通过 MCP 协议调用视觉 API 分析图片

[![License: MIT](https://img.shields.io/badge/license-MIT-%23000)](LICENSE)

</div>

---

## Install

```bash
npm install -g github:EDMOK/claude-vision-mcp
```

> 无需 TypeScript 环境，预编译单文件，安装即用。

## Configure

```bash
export VISION_API_KEY=sk-...
export VISION_BASE_URL=https://api.openai.com/v1   # 可选
export VISION_MODEL=gpt-4o                          # 可选
```

## MCP Config

在 `~/.claude.json` 的 `mcpServers` 中添加：

```json
"vision": {
  "command": "claude-vision-mcp",
  "env": {
    "VISION_API_KEY": "sk-...",
    "VISION_BASE_URL": "https://api.openai.com/v1",
    "VISION_MODEL": "gpt-4o"
  },
  "type": "stdio"
}
```

Windows 用户：

```json
"vision": {
  "command": "cmd",
  "args": ["/c", "claude-vision-mcp"],
  "env": {
    "VISION_API_KEY": "sk-...",
    "VISION_BASE_URL": "https://api-inference.modelscope.cn/v1",
    "VISION_MODEL": "Qwen/Qwen3.5-27B"
  },
  "type": "stdio"
}
```

重启 MCP 客户端即可调用 `vision_analyze` 工具。

## Params

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `image_type` | string | ✅ | `url` / `base64` / `file` / `clipboard` |
| `image_data` | string | ❌ | URL、Base64 或文件路径 |
| `media_type` | string | ❌ | Base64 时需要，如 `image/png` |
| `prompt` | string | ❌ | 自定义提示词 |
| `max_tokens` | number | ❌ | 默认 2048 |

## Examples

```
vision_analyze(image_type: "url", image_data: "https://example.com/photo.jpg")
vision_analyze(image_type: "file", image_data: "/Users/me/screenshot.png")
vision_analyze(image_type: "clipboard")
```

## Env

| 变量 | 必填 | 默认值 | 说明 |
|------|------|--------|------|
| `VISION_API_KEY` | ✅ | — | OpenAI 兼容 API 密钥 |
| `VISION_BASE_URL` | ❌ | `https://api.openai.com/v1` | API 地址 |
| `VISION_MODEL` | ❌ | `gpt-4o` | 视觉模型 |
| `VISION_TOOL_DISABLED` | ❌ | `false` | 设为 `true` 隐藏工具 |

## Requirements

- Node.js >= 20
- 剪贴板功能仅 Windows + PowerShell

## Changelog

### 2026-05-29 — v1.1.0

- **工具 description 全面重写** — 6 部分结构化指引（功能说明、必要性、使用场景、5 种 image_type 决策树、prompt 用法、输出说明），让纯文本模型更精准地调用视觉能力
- **剪贴板能力明确告知模型** — 模型现在知道 `clipboard` 模式可直接读取 Windows 系统剪贴板，不再要求用户手动粘贴图片
- **构建输出改为 CommonJS** — 修复 OpenAI SDK 在 ESM 下动态 `require` 的兼容问题
- **参数 description 精简优化** — 每个参数配场景化指引，降低模型误用率

## License

MIT
