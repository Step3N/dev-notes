# CUDA + cuDNN 安装配置

## 前提条件

**必须拥有 NVIDIA GPU**：

| 系统 | 检查命令 | 说明 |
|------|---------|------|
| **Linux** | `nvidia-smi` | 驱动正常则显示 GPU 信息 |
| **Windows** | `nvidia-smi` (CMD/PowerShell) | 需已安装 NVIDIA 驱动 |
| **macOS (Intel)** | 部分旧款 Mac 有 NVIDIA GPU（2016 前），否则无 | 建议直接放弃 |
| **macOS (Apple Silicon)** | **没有 NVIDIA GPU** | 请使用 MPS 或 CPU |

> **macOS Apple Silicon 用户**：CUDA 不可用。替代方案见文末。

### nvidia-smi 输出解读

```bash
nvidia-smi
# 输出中 "CUDA Version: 12.4" 表示驱动支持的**最高** CUDA 版本
# 不等于已安装的 CUDA Toolkit 版本！
```

## 安装 CUDA Toolkit

### Linux (Ubuntu/Debian) — 推荐 deb 包方式

以 Ubuntu 22.04 + CUDA 12.4 为例：

```bash
# 访问 https://developer.nvidia.com/cuda-downloads 选择系统版本
# 复制对应命令执行，以下以 Ubuntu 22.04 + CUDA 12.4 为例：
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update

# 安装 CUDA Toolkit 12.4
sudo apt install cuda-toolkit-12-4
```

> 其他 Ubuntu 版本将 `ubuntu2204` 替换为对应版本，如 `ubuntu2004`、`ubuntu2404`。

### Linux — Runfile 方式（手动，精确控制版本）

```bash
# 从 https://developer.nvidia.com/cuda-downloads 选择对应平台获取下载链接
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
sudo sh cuda_12.4.0_550.54.14_linux.run
# 取消勾选 Driver（如果已有驱动），只安装 Toolkit
```

### Windows

1. 访问 https://developer.nvidia.com/cuda-downloads
2. 选择操作系统版本，下载 `.exe` 安装包
3. 双击运行，选择 **Express**（推荐）或 **Custom**
4. 安装程序会自动添加 `CUDA_PATH` 环境变量

验证：

```powershell
nvcc --version
# 或检查环境变量
echo $env:CUDA_PATH
```

## 安装 cuDNN

需要注册 [NVIDIA Developer 账号](https://developer.nvidia.com/)。

### Linux

```bash
# 下载 cuDNN for CUDA 12.x（.tar.xz 文件）
tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda12-archive.tar.xz

# 复制文件到 CUDA 目录
sudo cp cudnn-*/include/cudnn*.h /usr/local/cuda-12.4/include
sudo cp cudnn-*/lib/libcudnn* /usr/local/cuda-12.4/lib64
sudo chmod a+r /usr/local/cuda-12.4/include/cudnn*.h /usr/local/cuda-12.4/lib64/libcudnn*

# 验证
cat /usr/local/cuda-12.4/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```

### Windows

1. 下载 cuDNN `.exe` 或 `.zip` 包
2. 解压后得到 `bin/`、`include/`、`lib/` 三个文件夹
3. 复制到 `%CUDA_PATH%` 目录下（通常为 `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\`）

验证：

```powershell
# 检查 cuDNN 版本
Get-ChildItem "$env:CUDA_PATH\lib\x64\cudnn*.dll"
```

## CUDA 版本与 PyTorch 兼容性

| PyTorch 版本 | 支持 CUDA | 安装命令 |
|-------------|----------|---------|
| 2.5.x | 12.4 / 12.1 | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124` |
| 2.4.x | 12.4 / 12.1 | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu124` |
| 2.3.x | 12.1 / 11.8 | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121` |
| 2.0.x | 11.8 / 11.7 | `pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118` |

> **重要**：安装前先访问 [pytorch.org](https://pytorch.org) 查看最新兼容矩阵。安装的 CUDA Toolkit 版本应 ≥ PyTorch 构建目标版本（向上兼容）。

### 验证 PyTorch 能否使用 CUDA

```bash
python -c "import torch; print(f'CUDA: {torch.cuda.is_available()}, Device: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else \"N/A\"}')"
```

## nvidia-smi vs nvcc 的区别

```
nvidia-smi 显示的 CUDA Version → 驱动支持的**最大** CUDA 版本
nvcc --version 显示的版本    → 实际安装的 CUDA Toolkit 版本
```

两者可以不同。例如驱动支持 12.4，但只装了 CUDA Toolkit 11.8，PyTorch 也能正常工作。

## 常见问题

### nvcc: command not found

**Linux**：未添加 PATH，或安装未完成。

```bash
export PATH=/usr/local/cuda/bin:$PATH
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.zshrc
```

如果 `ls /usr/local/cuda` 不存在，说明未成功安装，重新执行安装步骤。

### 多版本 CUDA 冲突

**Linux** 使用 `update-alternatives` 管理：

```bash
sudo update-alternatives --install /usr/local/cuda cuda /usr/local/cuda-12.4 1
sudo update-alternatives --install /usr/local/cuda cuda /usr/local/cuda-11.8 2
sudo update-alternatives --config cuda  # 选择默认版本
```

**Windows**：手动调整 `CUDA_PATH` 环境变量的值。

### WSL2 中使用 CUDA

1. **Windows 宿主机**安装 NVIDIA 驱动（WSL2 复用此驱动）
2. **WSL2 内**安装 CUDA Toolkit：

```bash
# 访问 https://developer.nvidia.com/cuda-downloads 选择 WSL-Ubuntu
# 复制对应命令执行
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt update
sudo apt install cuda-toolkit-12-4
```

验证：

```bash
nvidia-smi  # 应该能看到 GPU
nvcc --version
```

### macOS Apple Silicon — 没有 CUDA

```bash
# 检查 MPS 是否可用
python -c "import torch; print(f'MPS available: {torch.backends.mps.is_available()}')"

# 使用 MPS 加速
# device = torch.device('mps')
```

> Apple Silicon Mac 应使用 `torch.device("mps")` 而非 `cuda`。

### conda 环境下 CUDA 不生效

Conda 环境中建议用 `conda install` 安装 CUDA 相关包，避免与系统 CUDA 冲突：

```bash
conda install -c conda-forge cudatoolkit=12.4 cudnn=8.9
```

此方式不需要单独安装 CUDA Toolkit，适合在 Conda 环境中管理。

## 完全卸载 CUDA

**Linux**：

```bash
sudo apt --purge remove "*cuda*" "*cudnn*"
sudo apt autoremove
sudo rm -rf /usr/local/cuda*
```

**Windows**：控制面板 → 卸载程序 → 找到所有 NVIDIA CUDA 相关条目逐一卸载。
