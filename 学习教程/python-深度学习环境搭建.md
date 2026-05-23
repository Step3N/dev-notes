# [链路] Python 深度学习环境搭建

> 从零到一，装好 Python 和 PyTorch，GPU 跑通深度学习代码

**适用平台**：macOS / Windows / Linux
**预估耗时**：30-60 分钟
**前置条件**：
- NVIDIA GPU（Windows/Linux）或 Apple Silicon Mac（macOS）
- 没有 GPU 也能走通，但用 CPU 训练较慢

---

## 链路总览

| 步骤 | 内容 | 耗时 |
|------|------|------|
| Step 1 | 安装 Python | 5 min |
| Step 2 | 配置 pip 镜像源 | 1 min |
| Step 3 | 安装 Miniconda | 5 min |
| Step 4 | 创建 conda 环境 | 2 min |
| Step 5 | 检查 GPU 并安装 CUDA | 10 min |
| Step 6 | 安装 PyTorch | 3 min |
| Step 7 | 验证 GPU 可用 | 1 min |
| Step 8 | 安装常用深度学习包 | 3 min |
| Final | 跑一个完整的训练测试 | 2 min |

---

## Step 1: 安装 Python

```bash
# macOS
brew install python@3.12

# Windows — 从 https://python.org 下载安装包，勾选 "Add to PATH"
# Linux (Ubuntu/Debian)
sudo apt update && sudo apt install python3.12 python3.12-venv -y
```

验证：

```bash
python3 --version
# 输出: Python 3.12.x
```

> 📖 详见：[安装 Python](../安装软件/python.md) — 三平台差异和排错

---

## Step 2: 配置 pip 镜像源

国内网络推荐使用清华镜像，加速包下载：

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip config list  # 验证
```

确认输出包含 `global.index-url`。

其他可选镜像：

```bash
# 阿里云
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 中科大
pip config set global.index-url https://pypi.mirrors.ustc.edu.cn/simple/
```

> 📖 详见：[pip 换源](../配置调优/pip-换源.md)

---

## Step 3: 安装 Miniconda

```bash
# macOS
brew install --cask miniconda

# Linux
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# 按提示完成，最后选择 yes 运行 conda init

# Windows — 从 https://docs.conda.io/en/latest/miniconda.html 下载 exe 安装
```

初始化 conda（如果安装时未自动执行）：

```bash
conda init "$(basename "$SHELL")"
```

重启终端后验证：

```bash
conda --version  # 预期: conda 24.x.x
```

> 📖 详见：[Miniconda 安装使用](../安装软件/miniconda.md)

---

## Step 4: 创建 conda 环境

```bash
conda create -n dl python=3.12 -y
conda activate dl
```

验证：

```bash
python --version        # 预期: Python 3.12.x
which python            # 预期路径应在 miniconda3/envs/dl/bin
```

**为什么用 conda 环境？**
- 隔离依赖：不同项目可以用不同 Python / CUDA / 包版本
- 深度学习包经常依赖特定版本的 CUDA runtime，conda 能自动处理
- 切换项目时 `conda deactivate` 再 `conda activate <新环境>` 即可

---

## Step 5: 检查 GPU 并安装 CUDA

> ⚠️ macOS 用户请跳过此步。Apple Silicon 不支持 CUDA，PyTorch 会使用 MPS 加速（Step 7 验证）

检查 NVIDIA 驱动和 CUDA 版本：

```bash
nvidia-smi
```

输出示例：

```
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 545.23.08    Driver Version: 545.23.08    CUDA Version: 12.3     |
+---------------------------------------------------------------------------------------+
```

关键信息：**CUDA Version**。如果 >= 12.x，驱动已满足需求，只需安装 CUDA Toolkit。

如果 `nvidia-smi: command not found` → 先装 NVIDIA 驱动。
如果 CUDA Version 偏低（< 11.8）→ 升级驱动。

### 安装 CUDA Toolkit

```bash
# 访问 https://developer.nvidia.com/cuda-downloads 选择系统版本
# 以下以 Ubuntu 22.04 + CUDA 12.4 为例：
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update
sudo apt install cuda-toolkit-12-4 -y
```

```powershell
# Windows — 从 https://developer.nvidia.com/cuda-downloads 下载 exe
# 选择对应版本，默认安装即可
```

验证：

```bash
nvcc --version
# 输出: Cuda compilation tools, release 12.4, V12.4.xxx
```

> 📖 详见：[CUDA 配置](../配置调优/cuda-配置.md)

---

## Step 6: 安装 PyTorch

去 [pytorch.org](https://pytorch.org) 获取最新安装命令，或从下方选择。

**CUDA 12.x（推荐）**：

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

**CUDA 11.8**：

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

**CPU only / Apple Silicon MPS**：

```bash
pip install torch torchvision torchaudio
```

> 📖 详见：[PyTorch 安装](../安装软件/pytorch.md)

---

## Step 7: 验证 GPU 可用

```bash
python -c "
import torch
print(f'PyTorch: {torch.__version__}')
print(f'CUDA: {torch.cuda.is_available()}')
if torch.cuda.is_available():
    print(f'GPU: {torch.cuda.get_device_name(0)}')
    x = torch.rand(3, 3).cuda()
    print(f'Tensor on GPU: {x}')
print(f'MPS: {torch.backends.mps.is_available()}')
"
```

预期输出（NVIDIA GPU）：

```
PyTorch: 2.4.0
CUDA: True
GPU: NVIDIA GeForce RTX 4090
Tensor on GPU: tensor([[0.123, 0.456, 0.789], ...], device='cuda:0')
MPS: False
```

预期输出（Apple Silicon Mac）：

```
PyTorch: 2.4.0
CUDA: False
MPS: True
```

如果 `CUDA: False` 且你有 NVIDIA GPU → 看下方常见问题。

---

## Step 8: 安装常用深度学习包

```bash
pip install torch torchvision torchaudio    # 如 Step 6 已装可跳过
pip install transformers datasets            # HuggingFace 生态
pip install tensorboard                      # 训练可视化
pip install jupyter scikit-learn matplotlib  # 数据处理与探索
pip install accelerate                       # 分布式训练工具
pip install einops                           # 张量运算语法糖
```

批量验证：

```bash
python -c "
import transformers, datasets, tensorboard, sklearn, accelerate
print('All packages imported successfully')
"
```

---

## 最终验证：跑一个完整的训练测试

保存为 `test_dl.py`：

```python
import torch
import torch.nn as nn

device = torch.device(
    'cuda' if torch.cuda.is_available()
    else 'mps' if torch.backends.mps.is_available()
    else 'cpu'
)
print(f'Using: {device}')

model = nn.Sequential(
    nn.Linear(784, 256),
    nn.ReLU(),
    nn.Linear(256, 10),
).to(device)

x = torch.randn(64, 784).to(device)
y = model(x)
print(f'Forward pass: {y.shape}')  # Expected: torch.Size([64, 10])

loss_fn = nn.CrossEntropyLoss()
target = torch.randint(0, 10, (64,)).to(device)
loss = loss_fn(y, target)
loss.backward()
print(f'Loss: {loss.item():.4f}')
print('✅ 深度学习环境搭建成功！')
```

运行：

```bash
python test_dl.py
```

预期输出（任意平台）：

```
Using: cuda  (或 mps / cpu)
Forward pass: torch.Size([64, 10])
Loss: 2.30xx
✅ 深度学习环境搭建成功！
```

---

## 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| `torch.cuda.is_available()` 返回 `False` | 装了 CPU 版 PyTorch | `pip uninstall torch torchvision torchaudio`，重新安装 CUDA 版 |
| `nvidia-smi: command not found` | 未装 NVIDIA 驱动 | 去 [nvidia.com/drivers](https://nvidia.com/drivers) 下载对应驱动 |
| 安装 PyTorch 下载慢 | 未配置镜像 / 用了 conda 默认源 | `pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple` |
| conda 环境占用空间大（~3GB） | conda 包含大量预编译包 | 改用 `micromamba` 或 pip + venv |
| macOS 训练速度慢 | 无独立 GPU | 使用 MPS 加速；或使用云 GPU（Colab、AutoDL） |
| `CUDA driver version is insufficient` | 驱动版本低于 PyTorch 要求 | 升级驱动，或安装低 CUDA 版 PyTorch |
| pip install 报 `externally-managed-environment` | 系统 Python 禁止 pip 安装 | 确保在 conda 环境或 venv 中操作 |

---

## 下一步推荐

- [AI 编程环境搭建](./ai-编程环境搭建.md) — 配置 VS Code + Continue + Copilot
- [Docker 项目运行](./docker-项目运行.md) — 容器化你的深度学习项目
- [Jupyter 安装使用](../安装软件/jupyter.md) — 交互式编程环境配置
