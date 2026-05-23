# [链路] AI 编程环境搭建

> 配置 AI 编程助手，让你每天写代码效率翻倍

**适用平台**：macOS / Windows / Linux
**预估耗时**：20-40 分钟
**前置条件**：
- 已安装 VS Code（或兼容 IDE）
- 有 GitHub / Google / 手机号账号（用于注册 API 服务）

---

## 链路总览

| 步骤 | 内容 | 耗时 |
|------|------|------|
| Step 1 | 获取 API Key | 5-10 min |
| Step 2 | 配置环境变量 | 2 min |
| Step 3 | VS Code + Continue 扩展 | 5 min |
| Step 4 | Cursor IDE（备选） | 5 min |
| Step 5 | 本地部署 Ollama（可选） | 10 min |
| Step 6 | Prompt 实战模板 | 3 min |
| Step 7 | 选择适合你的方案 | 1 min |

---

## Step 1: 获取 API Key

AI 编程助手通过 API 调用大模型。以下是三个主流选择：

### OpenAI（GPT-4o / GPT-4.1）

```bash
# 访问 chat.openai.com → 右上角头像 → Settings → API Keys → Create new secret key
# 保存生成的 sk-xxx 字符串
```

### Anthropic Claude（Sonnet / Haiku）

```bash
# 访问 console.anthropic.com → API Keys → Create Key
# 保存生成的 sk-ant-xxx 字符串
```

### DeepSeek（中文友好，近乎免费）

```bash
# 访问 platform.deepseek.com/api_keys → 创建 API Key
# DeepSeek 注册送 500 万 tokens，用完即止
```

**对比总结**：

| 服务 | 初始额度 | 代码能力 | 中文友好 | 网络要求 |
|------|---------|---------|---------|---------|
| OpenAI GPT-4o | 付费，约 $5/月起步 | ⭐⭐⭐⭐ | 一般 | 需代理 |
| Claude Sonnet | 付费，约 $5/月起步 | ⭐⭐⭐⭐⭐ | 一般 | 需代理 |
| DeepSeek | 免费 500万 tokens | ⭐⭐⭐⭐ | ✅ 优秀 | 国内直连 |

> 📖 详见 [国产大模型API](../AI效率/国产大模型.md) / [OpenAI](../AI效率/OpenAI.md) / [Claude](../AI效率/Claude.md)

---

## Step 2: 配置 API Key 到环境变量

将 Key 写入 shell 配置文件，避免每次粘贴。

### macOS / Linux

```bash
# 编辑 ~/.zshrc（如果你用 bash，则编辑 ~/.bashrc 或 ~/.bash_profile）
echo 'export OPENAI_API_KEY="sk-xxxx"' >> ~/.zshrc
echo 'export ANTHROPIC_API_KEY="sk-ant-xxxx"' >> ~/.zshrc
echo 'export DEEPSEEK_API_KEY="sk-xxxx"' >> ~/.zshrc

# 重新加载配置
source ~/.zshrc
```

### Windows PowerShell

```powershell
# 以管理员身份打开 PowerShell，编辑 $PROFILE
notepad $PROFILE

# 在文件中添加：
# $env:OPENAI_API_KEY = "sk-xxxx"
# $env:ANTHROPIC_API_KEY = "sk-ant-xxxx"
# $env:DEEPSEEK_API_KEY = "sk-xxxx"

# 保存后执行：
. $PROFILE
```

### 验证

```bash
echo $OPENAI_API_KEY
# 输出应为 sk-...，如果为空则配置未生效
```

> 💡 安全提示：**永远不要**将 API Key 提交到 Git 仓库。如果误提交，立即去官网吊销该 Key。

> 📖 详见 [API Key 管理最佳实践](../AI效率/API-Key管理.md)

---

## Step 3: VS Code + Continue 扩展

Continue 是 VS Code 上最强大的开源 AI 编程插件，支持多模型切换、上下文管理、内联编辑。

### 安装

```bash
# 方法一：VS Code 扩展面板搜索 "Continue" → 安装
# 方法二：终端直接安装（macOS / Linux）
code --install-extension continue.continue
```

### 配置模型

打开 Continue 扩展 → 点击齿轮图标 → 选择 `config.json`。

**最小配置示例**（只连 DeepSeek）：

```json
{
  "models": [{
    "title": "DeepSeek",
    "provider": "openai",
    "model": "deepseek-chat",
    "apiKey": "sk-你的key"
  }]
}
```

**完整配置示例**（多模型切换）：

```json
{
  "models": [
    {
      "title": "DeepSeek",
      "provider": "openai",
      "model": "deepseek-chat",
      "apiKey": "sk-你的key"
    },
    {
      "title": "Claude Sonnet",
      "provider": "anthropic",
      "model": "claude-sonnet-4-20250514",
      "apiKey": "sk-ant-你的key"
    }
  ],
  "tabAutocompleteModel": {
    "title": "DeepSeek",
    "provider": "openai",
    "model": "deepseek-chat",
    "apiKey": "sk-你的key"
  }
}
```

### 使用

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 内联编辑 | `Cmd+I` / `Ctrl+I` | 选中代码直接修改 |
| 对话框 | `Cmd+L` / `Ctrl+L` | 全功能对话聊天 |
| 自动补全 | 输入时自动 | 类似 Copilot 的 Tab 补全 |

> 📖 详见 [Continue 扩展使用](../学习教程/continue.md)

---

## Step 4: Cursor IDE（备选）

如果你想要更原生的 AI 编程体验，可以直接使用 Cursor —— 基于 VS Code 的 AI-first IDE。

### 安装

```bash
# 访问 cursor.com → Download → 安装
# 安装后首次启动，选择 "Import VS Code Settings" 一键迁移
```

### 核心功能

| 功能 | 快捷键 | 说明 |
|------|--------|------|
| 内联编辑 | `Cmd+K` | 选中代码描述修改目标 |
| 对话框 | `Cmd+L` | 上下文对话聊天 |
| Composer | `Cmd+Shift+I` | 多文件编辑工作流 |
| Agent 模式 | 对话框选 Agent | 自动读项目上下文、执行命令 |

### VS Code + Continue vs Cursor

| 维度 | VS Code + Continue | Cursor |
|------|-------------------|--------|
| 费用 | 免费（仅付 API 费） | $20/月 |
| 模型自由 | ✅ 任意切换 | ⚠️ 有限制 |
| Tab 补全质量 | 中 | 高 |
| 上下文理解 | 依赖手动 @ 文件 | 自动索引 |

> 📖 详见 [Cursor 使用笔记](../AI效率/cursor.md)

---

## Step 5（可选）: 本地部署 Ollama + 免费模型

隐私敏感或离线场景下，在本地运行开源模型。

### 安装 Ollama

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# 从 ollama.com/download 下载安装包
```

### 拉取模型并测试

```bash
# 推荐模型（按需选一个，按大小排序）
ollama pull qwen2.5:0.5b     # 0.5B，极速，适合补全
ollama pull qwen2.5:3b       # 3B，平衡速度和效果
ollama pull qwen2.5:7b       # 7B，中文优秀，推荐
ollama pull deepseek-coder   # 6.7B，代码专用

# 测试对话
ollama run qwen2.5:7b
```

测试成功后，输入 `/bye` 退出。

### 接入 Continue

在 Continue `config.json` 中添加：

```json
{
  "models": [{
    "title": "Qwen2.5 (本地)",
    "provider": "ollama",
    "model": "qwen2.5:7b"
  }]
}
```

### 使用 GPU 加速

```bash
# macOS（M 芯片自带 Metal 加速，无需配置）
# Linux（NVIDIA GPU）
ollama pull qwen2.5:7b  # 自动检测 CUDA

# 检查是否在用 GPU
ollama ps
```

> 📖 详见 [Ollama 安装使用](../安装软件/ollama.md)

---

## Step 6: Prompt 实战模板

用好 AI 编程助手，关键是写对 Prompt。以下 5 个模板覆盖日常高频场景，直接复制粘贴修改。

### 1. 生成代码

~~~
Write a Python function to {描述功能}.
Requirements:
- Include type hints
- Include Google-style docstring
- Handle edge cases gracefully
- Return typed result
~~~

**示例**：

~~~
Write a Python function to parse a JSON file and extract all email addresses.
Requirements:
- Include type hints
- Include Google-style docstring
- Handle file not found and malformed JSON
- Return List[str]
~~~

### 2. 调试 Bug

~~~
This code produces the error below. Find the root cause and fix it.

Code:
```python
{paste code here}
```

Error:
```
{paste error here}
```
~~~

### 3. 重构代码

~~~
Refactor this function to be more readable and maintainable:
- Reduce cyclomatic complexity
- Use early returns
- Extract helper functions where appropriate
- Keep the same public API

```python
{paste code here}
```
~~~

### 4. 写测试

~~~
Write pytest tests for this function. Cover:
- Normal cases
- Edge cases (empty input, None, boundary values)
- Error / exception cases

```python
{paste code here}
```

Use pytest fixtures where appropriate.
~~~

### 5. 生成 Commit Message

~~~
Generate a conventional commit message for this diff. Use format:
`<type>(<scope>): <description>`

Types: feat, fix, refactor, test, docs, chore, style

Diff:
```
{paste git diff here}
```
~~~

---

## Step 7: 选择适合你的方案

| 方案 | 费用 | 代码质量 | 隐私 | 离线 | 推荐场景 |
|------|------|---------|------|------|---------|
| Claude + Continue | 按量付费 | ⭐⭐⭐⭐⭐ | ❌ | ❌ | 日常开发首选 |
| DeepSeek + Continue | 近乎免费 | ⭐⭐⭐⭐ | ❌ | ❌ | 国内用户首选 |
| GPT-4o + Continue | $5-20/月 | ⭐⭐⭐⭐ | ❌ | ❌ | 综合平衡方案 |
| Cursor | $20/月 | ⭐⭐⭐⭐⭐ | ❌ | ❌ | 追求 AI 原生体验 |
| Ollama 本地模型 | 免费 | ⭐⭐⭐ | ✅ | ✅ | 隐私敏感/离线 |

**推荐策略**：

1. **国内用户优先**：DeepSeek + Continue，零成本上手
2. **追求最佳代码质量**：Claude + Continue，物有所值
3. **混合使用**：日常用 DeepSeek，复杂任务切 Claude（Continue 支持一键切换模型）
4. **隐私/离线**：Ollama + Qwen2.5，虽然不如云端模型，但数据不出本机

---

## 常见问题

| 问题 | 解决 |
|------|------|
| API Key 泄露到 Git | 使用 `.env` 文件 + `.gitignore` 排除，参考 [API Key 管理最佳实践](../AI效率/API-Key管理.md) |
| 本地模型太慢 | 换小模型（Qwen2.5:3B 或 Phi-3-mini），或使用 GPU 加速 |
| Continue 连不上 API | 检查：① 网络代理是否正常 ② API Key 是否正确 ③ 账户余额是否充足 |
| Cursor 中文乱码 | 设置中搜索 `terminal.integrated.fontFamily`，改为支持中文的字体如 `"Menlo, 'Noto Sans CJK SC', monospace"` |
| Ollama 模型下载慢 | 设置 `OLLAMA_HOST` 或使用镜像源，参考 [Ollama 安装使用](../安装软件/ollama.md) |

---

## 下一步

- [Python 深度学习环境搭建](./python-深度学习环境搭建.md) — 配置 GPU 训练环境
- [Continue 扩展使用](../学习教程/continue.md) — 深入配置 Continue 的全部功能
- [Cursor 使用笔记](../AI效率/cursor.md) — 探索 Cursor 的高级功能

---

> **TL;DR**：注册 DeepSeek → 拿 Key → `export DEEPSEEK_API_KEY="sk-xxx"` → VS Code 装 Continue → 配置 DeepSeek 模型 → 开始用 `Cmd+I` 写代码。10 分钟搞定，零成本。
