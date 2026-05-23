# Aider — 终端 AI 编程助手

Aider 是一个终端工具，直接在命令行里和 AI 对话，自动编辑你的代码并用 git 管理每次改动。

官网：[aider.chat](https://aider.chat)

---

## 安装

```bash
# pip
pip install aider-chat

# brew（macOS）
brew install aider

# 或者直接运行（无需安装）
pipx install aider-chat
```

依赖：需要 `git` 在你的 PATH 中。

## 配置

设置 API Key 环境变量（三选一）：

```bash
# OpenAI
export OPENAI_API_KEY="sk-..."

# Anthropic
export ANTHROPIC_API_KEY="sk-ant-..."

# DeepSeek（免费的替代方案）
export DEEPSEEK_API_KEY="sk-..."
```

或者使用 `.env` 文件：

```bash
echo "OPENAI_API_KEY=sk-..." > .env
```

## 基本使用

```bash
# 在项目目录下运行
aider

# 或者指定模型
aider --model claude-sonnet-4-20250514

# 指定要编辑的文件
aider src/main.py src/utils.py
```

## 推荐的模型

| 模型 | 评价 | 成本 |
|------|------|------|
| Claude Sonnet 4 | 最佳代码编辑能力 | 💰 |
| GPT-4o | 全面均衡 | 💰 |
| DeepSeek | 免费方案，效果不错 | 免费 |
| Gemini 2.0 Flash | 快速且便宜 | 💰（便宜） |
| ollama/qwen2.5:7b | 本地方案，无需联网 | 免费 |

设置首选模型：

```bash
# 聊天中切换
/model claude-sonnet-4-20250514

# 或环境变量
export AIDER_MODEL=claude-sonnet-4-20250514
```

## 实战示例

```bash
# 在项目目录运行
$ aider

# 然后输入：
> 给 user.py 的 User 类添加 email 验证方法，要求使用正则表达式校验邮箱格式

# Aider 会：
# 1. 读取 user.py
# 2. 修改代码
# 3. 自动 git commit
```

## 内置命令

| 命令 | 作用 |
|------|------|
| `/add <file>` | 添加文件到对话上下文 |
| `/read-only <file>` | 添加只读文件 |
| `/drop <file>` | 移除文件 |
| `/git <command>` | 执行 git 命令 |
| `/commit` | 提交当前改动 |
| `/undo` | 撤销上一次 AI 改动 |
| `/diff` | 查看当前改动 |
| `/model <name>` | 切换模型 |
| `/tokens` | 查看 token 用量 |
| `/help` | 查看帮助 |

## 核心特性

**Repo Map** — Aider 自动分析项目结构，生成仓库地图，让 AI 理解代码上下文。

**Voice 模式** — 说话输入需求：

```bash
aider --voice
```

需要 Whisper API（OpenAI），或设置本地 Whisper 服务。

**自动提交** — 每次修改后自动生成有意义的 commit message：

```
feat: add email validation to User model
```

可以通过 `/undo` 回退。

**架构图** — Aider 会维护一个项目结构图，帮助理解大型代码库：

```
src/
├── main.py
├── models/
│   ├── user.py
│   └── post.py
├── services/
│   └── auth.py
└── utils/
    └── validators.py
```

## 本地模型搭配

Aider 支持本地模型，适合无法联网的场景：

```bash
OLLAMA_API_BASE=http://localhost:11434 aider --model ollama/qwen2.5:7b
```

或 LM Studio：

```bash
export OPENAI_API_BASE=http://localhost:1234/v1
export OPENAI_API_KEY=not-needed
aider --model openai/qwen2.5-7b-instruct
```

> 注意：本地模型效果远不如云端模型。建议本地模型做简单任务，复杂重构用云端。

---

**一句话总结**：Aider 是把 AI 整合进 git 工作流的最佳工具。适合习惯终端、用 git 管理版本、希望 AI 实际改代码而不是给建议的开发者。
