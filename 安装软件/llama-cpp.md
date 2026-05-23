# llama.cpp — 高性能 C++ 推理引擎

llama.cpp 是底层推理引擎，Ollama 和 LM Studio 底层其实都在用它。相比前两者，它更接近硬件、更可控，但需要手动编译和配置。

---

## 安装编译

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
```

### CPU 编译（通用）

```bash
make -j$(nproc)
```

### GPU 加速编译

不同平台需要不同后端，编译时选择对应选项：

```bash
# macOS Apple Silicon（Metal）
cmake -B build -DGGML_METAL=ON
cmake --build build --config Release -j$(nproc)

# Linux NVIDIA（CUDA）
cmake -B build -DGGML_CUDA=ON
cmake --build build --config Release -j$(nproc)

# Windows / Linux AMD（Vulkan）
cmake -B build -DGGML_VULKAN=ON
cmake --build build --config Release -j$(nproc)

# Intel GPU（SYCL）
cmake -B build -DGGML_SYCL=ON
```
```

### macOS 上的注意事项

- M 系列芯片强烈建议开启 Metal，速度提升 3-5 倍
- 不需要额外安装 CUDA，Xcode Command Line Tools 自带 Metal 支持

## 模型下载

从 Hugging Face 下载 GGUF 格式模型：

```bash
# 从 Hugging Face 下载 GGUF 模型，例如：
# 访问 https://huggingface.co/Qwen/Qwen2.5-7B-Instruct-GGUF 获取最新下载链接
wget https://huggingface.co/Qwen/Qwen2.5-7B-Instruct-GGUF/resolve/main/qwen2.5-7b-instruct-q4_k_m.gguf

# 放到 models/ 目录
mkdir -p models
mv qwen2.5-7b-instruct-q4_k_m.gguf models/
```

## 运行推理

### 交互模式

```bash
./llama-cli \
  -m models/qwen2.5-7b-instruct-q4_k_m.gguf \
  -p "用 Python 写一个快速排序" \
  -n 1024 \
  -t 8 \
  --temp 0.7
```

### 聊天模式

```bash
./llama-cli \
  -m models/qwen2.5-7b-instruct-q4_k_m.gguf \
  -f \
  --chat-template chatml \
  -i
```

## 服务器模式（OpenAI 兼容 API）

```bash
./llama-server \
  -m models/qwen2.5-7b-instruct-q4_k_m.gguf \
  --port 8080 \
  --host 0.0.0.0 \
  -ngl 99  # GPU 层数，-1 = 全部
```

然后任何 OpenAI 客户端指向 `http://localhost:8080/v1`：

```python
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8080/v1", api_key="llama")
```

## 关键特性

| 特性 | 说明 |
|------|------|
| **GGUF 量化** | Q4_K_M 是推荐的平衡点 |
| **KV Cache 量化** | 降低长上下文的内存占用，启用 `--cache-type-k q8_0` |
| **Continuous Batching** | 服务器模式下，动态批量处理请求，提升吞吐量 |
| **Speculative Decoding** | 用小模型草稿+大模型验证，加速推理 |
| **Flash Attention** | 大上下文场景加速 |

## 常用参数速查

```bash
./llama-cli \
  -m <model.gguf>      # 模型文件路径
  -p <prompt>           # 输入提示
  -n <tokens>           # 最大生成 token 数
  -t <threads>          # CPU 线程数
  --temp <float>        # 温度 (0-2)
  --top-p <float>       # top-p 采样
  --repeat-penalty <f>  # 重复惩罚 (1.0 = 无)
  -c <ctx-size>         # 上下文大小 (默认 512)
  -ngl <layers>         # GPU 加速层数 (-1 = 全部)
  -b <batch-size>       # 批处理大小
```

## 性能调优建议

- **首轮生成慢**：`-c 8192` 预分配上下文，减少重新分配开销
- **内存不足**：降量化等级（Q4_K_M → Q3_K_M → Q2_K）
- **GPU 利用率低**：增加 batch size（`-b 2048`）
- **服务器高并发**：启用 continuous batching

---

**一句话总结**：Ollama 和 LM Studio 都是在 llama.cpp 外面包了一层。需要极致性能或定制参数时，直接操作 llama.cpp 最灵活。
