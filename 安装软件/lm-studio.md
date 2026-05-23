# LM Studio — 带 GUI 的本地模型运行器

LM Studio 是一个桌面应用，可以搜索、下载、运行本地大模型。相比 Ollama，它提供了图形界面和更直观的模型配置。

官网下载：[lmstudio.ai](https://lmstudio.ai)

---

## 安装

| 平台 | 方式 |
|------|------|
| macOS | 下载 .dmg 安装（Apple Silicon 原生支持） |
| Windows | 下载 .exe 安装 |
| Linux | ❌ 无原生支持（可用 Web 版或 Wine） |

## 工作流程

1. **下载安装** LM Studio
2. **搜索模型** — 在应用内搜索框搜 "Qwen2.5-7B-Instruct-GGUF"
3. **选择量化版本** — 右侧选择 GGUF 量化格式
4. **下载** — 点击下载按钮
5. **加载模型** — 左侧面板选择模型并设置参数
6. **启动服务器** — 点击 "Start Server"
7. **使用** — 聊天界面直接对话，或通过 API 接入其他工具

## 量化版本选择

| 量化 | 大小（7B） | 质量 | 场景 |
|------|-----------|------|------|
| Q2_K | ~3GB | 偏低 | 内存紧缺 |
| Q3_K_M | ~4GB | 还行 | 最低可用 |
| **Q4_K_M** | **~4.5GB** | **推荐** | **日常首选** |
| Q5_K_M | ~5.5GB | 较好 | 有足够内存 |
| Q8_0 | ~7GB | 接近无损 | 追求质量 |
| F16 | ~14GB | 原始精度 | 仅限高配 GPU |

> 一般推荐 Q4_K_M，体积和质量的最佳平衡。Q8_0 如果有 16GB+ 内存。

## API 服务

启动服务器后，LM Studio 在 `http://localhost:1234` 提供 OpenAI 兼容 API：

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:1234/v1",
    api_key="not-needed"
)
```

## 模型配置参数

在加载模型时可以调整：

- **Context Length (n_ctx)**：上下文窗口，默认 4096，可调至 8192 或更高（吃内存）
- **GPU Offload (n_gpu_layers)**：多少层交给 GPU 计算，-1 = 全部（推荐）
- **Threads (n_threads)**：CPU 线程数，默认自动检测

## 优缺点

| 优点 | 缺点 |
|------|------|
| 漂亮的 GUI 界面 | 比 Ollama 更占资源 |
| 一键下载模型（从 Hugging Face） | 没有原生 Linux 版 |
| 可视化参数调优 | 后台常驻进程 |
| 支持视觉模型（多模态） | 启动较慢 |
| 内置聊天历史 | 社区版功能有限制 |

---

## 适用场景

- **GUI 党**：不想碰命令行，LM Studio 是首选
- **模型探索者**：经常换模型试效果，图形界面方便操作
- **本地 API 服务**：启动 server 模式后，和 Ollama 用法一样

## 和 Ollama 对比

| | Ollama | LM Studio |
|--|--------|-----------|
| 安装 | 终端命令 | GUI 安装包 |
| 操作 | 命令行 | 图形界面 |
| 模型下载 | `ollama pull` | 内置搜索+下载 |
| API 端口 | 11434 | 1234 |
| 资源占用 | 轻量 | 较重 |
| Linux | ✅ 原生 | ❌ 无 |
| 多模态 | 部分支持 | ✅ 支持 |

---

**总结**：想要图形界面、方便试不同模型 → LM Studio。快速轻量、命令行习惯 → Ollama。两者不冲突，可以同时装。
