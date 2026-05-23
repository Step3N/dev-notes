# Miniconda 安装与使用

> Miniconda 是 Python 环境管理工具，特别适合数据科学和深度学习场景。比完整版 Anaconda 更轻量，只包含 conda + Python，需要的包自行安装。

**适用平台**：macOS / Windows / Linux
**前置条件**：无（从零开始）

## 安装

### macOS

```bash
# 方式一：brew 安装（推荐）
brew install --cask miniconda

# 方式二：手动下载 pkg 安装
# 访问 https://docs.anaconda.com/miniconda/ 下载 .pkg 文件双击安装

# 安装后初始化 shell
conda init "$(basename "$SHELL")"
```

### Windows

1. 访问 https://docs.anaconda.com/miniconda/ 下载 `.exe` 安装包
2. 双击运行，**务必勾选** "Add to PATH"
3. 安装完成后，打开 **PowerShell** 或 **CMD** 运行：

```powershell
conda init powershell
# 或
conda init cmd.exe
```

> **注意**：安装后需**重启终端**使 `conda` 命令生效。

### Linux

```bash
# 方式一：脚本安装（推荐）
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# 按提示同意许可，选择安装路径，最后选 yes 运行 conda init

```

### 验证安装

```bash
conda --version
which conda
```

## 常用命令

```bash
# 创建环境
conda create -n myenv python=3.12

# 激活 / 退出环境
conda activate myenv
conda deactivate

# 安装包
conda install numpy pandas

# 查看已安装的包
conda list

# 查看所有环境
conda env list

# 导出 / 导入环境
conda env export > environment.yml
conda env create -f environment.yml

# 删除环境
conda remove -n myenv --all

# 更新 conda 自身
conda update -n base conda
```

## Conda vs pyenv vs venv

| 工具 | 适用场景 | 非 Python 依赖 | 速度 | 推荐人群 |
|------|---------|---------------|------|---------|
| **Conda** | 数据科学、ML | ✅ 支持 CUDA、cuDNN、OpenBLAS 等 | 中等 | 数据科学家、ML 工程师 |
| **pyenv** | 纯 Python 版本管理 | ❌ 仅 Python | 快 | 通用 Python 开发者 |
| **venv** | 轻量隔离 | ❌ 仅 Python | 最快 | 所有 Python 开发者 |

> Conda 最大优势：可以管理 CUDA、cuDNN、MKL 等非 Python 二进制依赖。如果只做 Web 开发，venv 完全够用。

## 镜像源配置

修改 `~/.condarc` 添加国内镜像：

```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

一行命令设置：

```bash
# 清华源
conda config --set show_channel_urls yes
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --set channel_priority strict
```

> **中科大镜像** 替换 `mirrors.tuna.tsinghua.edu.cn` 为 `mirrors.ustc.edu.cn`。

## 常见问题

### conda init 不生效

```bash
# 重启终端，或手动 source
source ~/.zshrc          # macOS/Linux (zsh)
source ~/.bashrc         # Linux (bash)
. "$HOME/anaconda3/etc/profile.d/conda.sh"  # 临时加载
```

### 环境太大，安装慢

```bash
# 方案一：安装时跳过已满足的依赖
conda install --freeze-installed numpy

# 方案二：使用 mamba（并行下载，快很多）
conda install -n base conda-forge::mamba
mamba create -n myenv python=3.12 numpy pandas
```

### Windows 上 conda activate 找不到

确保从 **PowerShell** 或 **CMD** 运行（不是 Git Bash 或 WSL 的 bash），并已执行 `conda init`。

### conda 占用磁盘过大

```bash
# 清理缓存
conda clean --all
```

## 卸载 Miniconda

**macOS / Linux**：
```bash
rm -rf ~/miniconda3 ~/.condarc ~/.conda
# 手动删除 ~/.zshrc 或 ~/.bashrc 中的 conda init 块
```

**Windows**：控制面板 → 卸载程序 → Miniconda，然后删除 `%USERPROFILE%\.conda` 目录。
