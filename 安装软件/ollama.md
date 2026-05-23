# Ollama — 本地运行 LLM 最简单的方式

Ollama 是目前最流行的本地 LLM 运行工具，一条命令就能下载并运行模型，自带 OpenAI 兼容 API。

---

## 安装

```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# 从 https://ollama.com 下载安装包
```

## 首次运行

```bash
ollama run qwen2.5:7b
```

这条命令会自动下载模型并启动交互式对话。推荐首次使用的模型：

| 模型 | 参数 | 特点 |
|------|------|------|
| `qwen2.5:7b` | 7B | 中文英文都好，编程能力强 |
| `qwen2.5` | 7B (默认) | 同上，:7b 的别名 |
| `llama3.2:3b` | 3B | 轻量，英文好 |
| `deepseek-r1:7b` | 7B | 带推理链的模型 |

## 模型管理

```bash
ollama list            # 查看已下载的模型
ollama pull <model>    # 只下载不运行
ollama rm <model>      # 删除模型
ollama cp <src> <dst>  # 复制模型（改名）
```

## OpenAI 兼容 API

Ollama 启动后自动在 `http://localhost:11434` 提供 REST API，兼容 OpenAI 格式：

```
http://localhost:11434/v1/chat/completions
```

任何支持 OpenAI API 的工具，只需改 base URL 就能用本地模型：

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # 随便填，Ollama 不验证
)

response = client.chat.completions.create(
    model="qwen2.5:7b",
    messages=[{"role": "user", "content": "你好"}]
)
print(response.choices[0].message.content)
```

## 自定义 Modelfile

通过 Modelfile 调整模型参数，创建定制版本：

```dockerfile
FROM qwen2.5:7b

PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER num_ctx 8192

SYSTEM "You are a senior software engineer. Answer concisely and provide code examples."
TEMPLATE """{{ if .System }}<|system|>
{{ .System }}
{{ end }}<|user|>
{{ .Prompt }}
<|assistant|>
"""
```

然后创建自定义模型：

```bash
ollama create my-coder -f Modelfile
ollama run my-coder
```

## GGUF 模型导入

从 Hugging Face 下载 GGUF 格式模型，直接导入 Ollama：

```bash
ollama create my-model -f Modelfile
```

其中 Modelfile 指向本地 GGUF 文件：

```dockerfile
FROM ./qwen2.5-7b-instruct-q4_k_m.gguf
```

---

## 实用技巧

- **后台运行**：Ollama 安装后默认以服务形式运行，无需手动启动
- **模型路径**：`~/.ollama/models/`，可以通过软链接转移到其他磁盘
- **Ollama Web UI**：搭配 Open WebUI（原 Ollama Web UI）获得 ChatGPT 般的体验
- **多模型同时运行**：Ollama 不支持多模型并行，一次只能跑一个

## 常见问题

| 问题 | 解决 |
|------|------|
| 模型下载慢 | 设置代理 `export HTTPS_PROXY=http://127.0.0.1:7890` |
| 内存不足 | 换小模型（3B、1.5B），或降低上下文长度（num_ctx=2048） |
| GPU 未使用 | macOS 自动使用 Metal，Linux 需安装 CUDA |

---

**一句话总结**：Ollama = 本地模型的一键解决方案。先装 Ollama，跑 `ollama run qwen2.5:7b`，然后把各种 AI 工具的 API 地址指向 `localhost:11434` 就行。
