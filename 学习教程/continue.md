# Continue — 开源 AI 代码助手

Continue 是一个开源的 IDE 插件，可以用任意 LLM 做代码补全和对话。最大的优势：支持本地模型，不依赖特定云服务商。

官网：[continue.dev](https://continue.dev)

---

## 安装

| IDE | 安装方式 |
|-----|---------|
| VS Code | 扩展市场搜 "Continue" (continue.continue) |
| JetBrains | 插件市场搜 "Continue" |
| 其他 | 暂不支持 |

## 配置模型

安装后在 `~/.continue/config.json` 配置模型：

```json
{
  "models": [
    {
      "title": "本地 Ollama",
      "provider": "ollama",
      "model": "qwen2.5:7b",
      "apiBase": "http://localhost:11434"
    },
    {
      "title": "GPT-4o",
      "provider": "openai",
      "model": "gpt-4o"
    },
    {
      "title": "Claude Sonnet",
      "provider": "anthropic",
      "model": "claude-sonnet-4-20250514"
    },
    {
      "title": "LM Studio",
      "provider": "openai",
      "model": "qwen2.5-7b-instruct",
      "apiBase": "http://localhost:1234/v1",
      "apiKey": "not-needed"
    }
  ]
}
```

> 可以同时配多个模型，聊天中随时切换。

## 快捷键

| 操作 | macOS | Windows/Linux |
|------|-------|--------------|
| 聊天侧边栏 | `Ctrl+L` | `Ctrl+L` |
| 内联编辑 | `Cmd+I` | `Ctrl+I` |
| 代码补全 | Tab（需要配置） | Tab（需要配置） |
| 接受补全 | `Tab` | `Tab` |
| 拒绝补全 | `Esc` | `Esc` |

## 核心功能

### 聊天侧边栏（Ctrl+L）

在侧边栏直接和 AI 对话。可以使用 @ 符号引用上下文：

| 快捷引用 | 作用 |
|---------|------|
| `@file` | 引用当前文件 |
| `@folder` | 引用整个文件夹 |
| `@codebase` | 引用整个代码库 |
| `@problems` | 引用编辑器错误列表 |
| `@web` | 搜索网络 |

示例：在聊天框输入 `@codebase 帮我重构这个模块`，Continue 会扫描整个项目。

### 内联编辑（Cmd+I）

选中代码后按 `Cmd+I`，直接在代码内部修改：

```
[选中代码] → Cmd+I → "添加错误处理并添加类型注解"
```

### 自定义 Slash Commands

在 `config.json` 中添加自定义指令：

```json
{
  "experimental": {
    "defaultModel": "GPT-4o",
    "commands": [
      {
        "name": "review",
        "description": "Code review selected code",
        "prompt": "Review the following code for bugs, security issues, and best practices:\n{{{input}}}"
      },
      {
        "name": "test",
        "description": "Write tests",
        "prompt": "Write comprehensive tests for the following code. Use the same testing framework as the project:\n{{{input}}}"
      },
      {
        "name": "explain",
        "description": "Explain selected code",
        "prompt": "Explain the following code step by step:\n{{{input}}}"
      }
    ]
  }
}
```

用法：选中代码后输入 `/review`、`/test`、`/explain`。

### 自动补全（Tab）

需要额外配置补全模型，推荐：

```json
{
  "tabAutocompleteModel": {
    "title": "Qwen2.5-Coder-7B",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b"
  }
}
```

或使用云端补全：

```json
{
  "tabAutocompleteModel": {
    "title": "Codestral",
    "provider": "mistral",
    "model": "codestral-latest",
    "apiKey": "your-mistral-key"
  }
}
```

## 和 GitHub Copilot 对比

| 对比 | Continue | GitHub Copilot |
|------|---------|---------------|
| 模型选择 | 任意模型，本地+云端 | 仅 OpenAI 模型 |
| 本地模型 | ✅ Ollama / LM Studio / llama.cpp | ❌ |
| 价格 | 免费 + 模型费用 | $10/月 |
| 开源 | ✅ Apache 2.0 | ❌ |
| 自定义指令 | ✅ 任意 slash command | ❌ 固定 |
| 补全质量 | 取决于配置的模型 | 较好（优化过） |
| IDE 支持 | VS Code + JetBrains | 多平台 |

---

## 最佳实践

1. **日常用云端模型**（GPT-4o / Claude）+ **补全用本地小模型**（Qwen2.5-Coder）
2. **开会或离线时**切换到本地模型
3. **代码审查**配置 `/review` 指令 + 本地模型，保护代码隐私
4. **多个模型切换**：简单改动用本地小模型，复杂重构切到 GPT-4o

## 配置文件示例（完整版）

```json
{
  "models": [
    { "title": "GPT-4o", "provider": "openai", "model": "gpt-4o" },
    { "title": "Ollama本地", "provider": "ollama", "model": "qwen2.5:7b" }
  ],
  "tabAutocompleteModel": {
    "title": "本地补全",
    "provider": "ollama",
    "model": "qwen2.5-coder:7b"
  },
  "experimental": {
    "commands": [
      { "name": "review", "description": "Code review", "prompt": "Review this code:\n{{{input}}}" },
      { "name": "fix", "description": "Fix issues", "prompt": "Fix bugs and issues:\n{{{input}}}" }
    ]
  }
}
```

---

**一句话总结**：Continue = 开源版 Copilot，但你可以用任何模型，包括你自己电脑上跑的。适合不想被绑定在单一厂商、或者需要本地运行的开发者。
