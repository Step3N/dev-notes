# Anthropic Claude API 笔记

Claude 以长上下文、安全性和高质量回答著称。API 与 OpenAI 不兼容，使用独立的 SDK。

---

## 认证

```bash
export ANTHROPIC_API_KEY="sk-ant-your-key-here"
```

Python SDK 会自动读取 `ANTHROPIC_API_KEY` 环境变量。

---

## 可用模型

| 模型 | 说明 | 上下文 |
|------|------|--------|
| Claude 3.5 Sonnet | 推荐，综合能力最强 | 200K |
| Claude 3.5 Haiku | 轻量快速，性价比高 | 200K |
| Claude 3 Opus | 旧旗舰，复杂任务 | 200K |
| Claude 4 Sonnet | 新一代旗舰 | 200K |

> 生产环境首选 Claude 3.5 Sonnet，日常轻量任务用 Haiku。

---

## Messages API

### 基础调用

```python
from anthropic import Anthropic

client = Anthropic()

response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "你好"}
    ]
)
print(response.content[0].text)
```

### 关键参数

| 参数 | 说明 |
|------|------|
| model | 模型名 |
| max_tokens | 最大输出 token（必填） |
| system | 系统提示词（独立参数，不在 messages 里） |
| messages | 对话消息列表 |
| temperature | 0-1，默认 1 |
| stream | 布尔值，是否流式 |

### System Prompt

与 OpenAI 不同，Claude 的 system prompt 是独立参数：

```python
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    system="你是一个专业的 Python 工程师，回答要简洁且附代码示例。",
    messages=[
        {"role": "user", "content": "如何用 async/await 处理并发？"}
    ]
)
```

### 流式输出

```python
stream = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[{"role": "user", "content": "写一首诗"}],
    stream=True
)
for event in stream:
    if event.type == "content_block_delta":
        print(event.delta.text, end="")
```

### Vision（图片）

```python
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "描述这张图片"},
            {"type": "image", "source": {
                "type": "base64",
                "media_type": "image/jpeg",
                "data": base64_image_string
            }}
        ]
    }]
)
```

### Tool Use

```python
response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[{"role": "user", "content": "纽约天气怎样？"}],
    tools=[{
        "name": "get_weather",
        "description": "获取天气",
        "input_schema": {
            "type": "object",
            "properties": {
                "location": {"type": "string"}
            },
            "required": ["location"]
        }
    }]
)
```

---

## 与 OpenAI 对比

| 维度 | Claude | OpenAI |
|------|--------|--------|
| 上下文 | 200K tokens | 128K (GPT-4o) |
| 价格 | 中间档 | 范围大 |
| 代码能力 | 强 | 强 |
| 中文能力 | 好 | 好 |
| System Prompt | 独立参数 | messages 里 |
| SDK | `anthropic` | `openai` |
| 登录平台 | console.anthropic.com | platform.openai.com |

---

## 限频

按使用层级（Tier）区分，免费 Tier 每分钟仅 5 次请求，Tier 4 可到 10K RPM。

查看当前限频：`https://console.anthropic.com/settings/limits`

---

## 价格参考

| 模型 | 输入 | 输出 |
|------|------|------|
| Claude 3.5 Sonnet | $3.00 | $15.00 |
| Claude 3.5 Haiku | $0.80 | $4.00 |
| Claude 3 Opus | $15.00 | $75.00 |

> 最新价格: https://www.anthropic.com/pricing

---

## 平台

全平台支持。`anthropic` SDK 需要 Python 3.8+。
