# OpenAI API 笔记

OpenAI 的 API 是目前最广泛使用的 LLM API，兼容性最好，很多国产模型也兼容它的格式。

---

## 认证

通过环境变量设置 API Key，避免写死在代码里：

```bash
export OPENAI_API_KEY="sk-your-key-here"
```

Python SDK 会自动读取 `OPENAI_API_KEY` 环境变量：

```python
from openai import OpenAI

client = OpenAI()  # 自动从环境变量读取 key
# 或显式传入：OpenAI(api_key="sk-...")
```

---

## 可用模型

| 模型 | 说明 | 上下文 |
|------|------|--------|
| GPT-4o | 最新旗舰，支持图文 | 128K |
| GPT-4o-mini | 轻量版，便宜，日常首选 | 128K |
| GPT-4-turbo | 旧旗舰，仍可用 | 128K |
| o1 | 推理模型，擅长复杂逻辑 | 200K |
| o3 | 最新推理模型，更强 | 200K |
| o4-mini | 轻量推理模型 | 200K |

> 最新模型列表: https://platform.openai.com/docs/models

---

## Chat Completions API

### 基础调用

```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "你是一个助手"},
        {"role": "user", "content": "你好"}
    ]
)
print(response.choices[0].message.content)
```

### 关键参数

| 参数 | 范围 | 说明 |
|------|------|------|
| temperature | 0-2 (默认 1) | 越高越随机，越低越确定 |
| max_tokens | 1-4096+ | 生成的最大 token 数 |
| top_p | 0-1 (默认 1) | 核采样，替代 temperature |
| frequency_penalty | -2 到 2 | 降低重复用词 |
| presence_penalty | -2 到 2 | 鼓励谈论新话题 |

### 流式输出

```python
stream = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "讲个笑话"}],
    stream=True
)
for chunk in stream:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
```

### Vision（图片输入）

```python
response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "这张图里有什么？"},
            {"type": "image_url", "image_url": {
                "url": "https://example.com/photo.jpg"
            }}
        ]
    }]
)
```

### Function Calling / Tool Use

```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "获取天气",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {"type": "string"}
            },
            "required": ["location"]
        }
    }
}]

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "北京天气如何？"}],
    tools=tools
)
```

---

## Token 计数

使用 `tiktoken` 库在本地计算 token 数：

```python
import tiktoken

enc = tiktoken.encoding_for_model("gpt-4o")
tokens = enc.encode("你好，世界")
print(len(tokens))  # token 数量
```

---

## 错误处理

```python
from openai import (
    RateLimitError,
    AuthenticationError,
    APITimeoutError,
    APIConnectionError,
)

try:
    response = client.chat.completions.create(...)
except RateLimitError:
    print("触发限频，等待重试...")
except AuthenticationError:
    print("API Key 无效或未设置")
except APITimeoutError:
    print("请求超时")
except APIConnectionError:
    print("网络连接失败")
```

---

## 限频 & 重试

遇到 429 或 5xx 用指数退避重试：

```python
import time
from openai import RateLimitError

max_retries = 3
for attempt in range(max_retries):
    try:
        response = client.chat.completions.create(...)
        break
    except RateLimitError:
        if attempt < max_retries - 1:
            time.sleep(2 ** attempt)  # 1s, 2s, 4s
```

---

## 价格参考

2025 年价格（每 1M tokens）：

| 模型 | 输入 | 输出 |
|------|------|------|
| GPT-4o | $2.50 | $10.00 |
| GPT-4o-mini | $0.15 | $0.60 |
| GPT-4-turbo | $10.00 | $30.00 |
| o1 | $15.00 | $60.00 |
| o3 | $10.00 | $40.00 |

> 最新价格: https://openai.com/api/pricing/

---

## 平台

全平台支持：macOS / Linux / Windows。Python SDK 需要 Python 3.8+。
