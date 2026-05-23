# PyTorch 安装指南

> **官方站点**: [pytorch.org](https://pytorch.org) — 始终以官网生成器为准，选择你的配置后复制命令。

---

## 常见安装命令

### CUDA 12.x（新 GPU 主流）

```bash
# pip
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124

# conda
conda install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

### CUDA 11.8（旧 GPU，兼容性更好）

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### CPU only（无 NVIDIA GPU，或 macOS）

```bash
pip install torch torchvision torchaudio
```

### macOS 带 MPS 加速（Apple Silicon）

```bash
pip install torch torchvision torchaudio
# 验证 MPS
python -c "import torch; print(torch.backends.mps.is_available())"
```

---

## 验证安装

```python
import torch

print(f"PyTorch version: {torch.__version__}")
print(f"CUDA available: {torch.cuda.is_available()}")
if torch.cuda.is_available():
    print(f"CUDA version: {torch.version.cuda}")
    print(f"GPU: {torch.cuda.get_device_name(0)}")
print(f"MPS available: {torch.backends.mps.is_available()}")
```

输出示例（RTX 4090 + CUDA 12.4）：
```
PyTorch version: 2.5.1
CUDA available: True
CUDA version: 12.4
GPU: NVIDIA GeForce RTX 4090
MPS available: False
```

---

## pip vs conda

| 维度 | pip | conda |
|------|-----|-------|
| 包更新速度 | ✅ 更快 | ❌ 较慢 |
| 非 Python 依赖 | ❌ 需手动处理 | ✅ 自动管理 |
| 环境隔离 | venv + pip | 内置 |
| 推荐场景 | 纯 Python / 最新特性 | 数据科学 / 深度学习 |

> 深度学习工作一般推荐 **pip**（更新快），但需要 CUDA/cuDNN 版本管理时 **conda** 更省心。

---

## 版本兼容性

PyTorch 主版本 ↔ 支持的最低 CUDA 版本（参考，以官网为准）：

| PyTorch | CUDA |
|---------|------|
| 2.5.x | 11.8 / 12.1 / 12.4 |
| 2.4.x | 11.8 / 12.1 |
| 2.3.x | 11.8 / 12.1 |
| 2.0.x | 11.7 / 11.8 |

查看已安装版本：
```bash
pip list | grep torch
```

---

## 常见问题

### `torch.cuda.is_available()` 返回 False

```bash
# 1. 检查驱动和 CUDA 工具包
nvidia-smi          # 驱动版本和支持的最高 CUDA
nvcc --version      # 实际安装的 CUDA 工具包版本

# 2. 确认安装的是 CUDA 版本（而不是 CPU-only）
pip list | grep torch
# 看到 +cpu 后缀说明装了 CPU 版本，需要重装 CUDA 版本
# 看到 +cu124 或 +cu118 才是正确的 CUDA 版本

# 3. 重新安装（卸载后重装）
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

### CUDA OOM（Out of Memory）

```bash
# 检查 GPU 占用
nvidia-smi

# 在代码中释放缓存
torch.cuda.empty_cache()

# 常用缓解措施
# - 减小 batch size
# - 梯度累积（gradient accumulation）
# - 使用混合精度训练（torch.cuda.amp）
# - 检查是否有残留进程占用显存
nvidia-smi --query-compute-apps=pid --format=csv,noheader | xargs kill -9
```

### macOS MPS 不可用

```bash
# 确认 macOS 版本 >= 12.3
sw_vers
# 确认 PyTorch 版本 >= 1.12
python -c "import torch; print(torch.__version__)"
```

---

## 快速安装决策树

```
有 NVIDIA GPU？
├─ 是 → 驱动支持 CUDA 12.x？→ 装 cu124 版本
│        └─ 驱动只支持 CUDA 11.x？→ 装 cu118 版本
└─ 否 → macOS Apple Silicon？→ 装 CPU 版（MPS 自动启用）
         └─ 其他 → 装 CPU 版
```

---

## 参考

- [PyTorch 官方安装指南](https://pytorch.org/get-started/locally/)
- [PyTorch 版本兼容矩阵](https://github.com/pytorch/pytorch/blob/main/RELEASE.md)
- [CUDA 工具包下载](https://developer.nvidia.com/cuda-toolkit-archive)
