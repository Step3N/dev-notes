# Python 安装指南

## macOS

### 方式一：Homebrew（推荐）

```bash
# 安装 Python 3
brew install python

# 验证
python3 --version
python3 -c "print('Hello')"
```

brew 安装的 `python3` 和 `pip3` 在 `/opt/homebrew/bin/`（Apple Silicon）或 `/usr/local/bin/`（Intel）。

### 方式二：官方安装包

从 [python.org](https://www.python.org/downloads/) 下载 `.pkg` 安装包，安装后会自动配置 `python3` 命令。

### 方式三：Xcode Command Line Tools（依赖）

```bash
xcode-select --install
```
> 很多 Python 编译依赖需要 Xcode CLI tools，建议先装。

### macOS 注意事项

- 系统自带的是 **Python 2**（`/usr/bin/python`），**不要删除**，系统组件依赖它。
- 手动安装的 Python 3 在 `/Library/Frameworks/Python.framework/`。
- `python` 可能指向 Python 2，始终用 `python3` 和 `pip3`。

---

## Windows

### 方式一：python.org 安装包（推荐）

1. 访问 [python.org/downloads](https://www.python.org/downloads/)
2. 下载最新 Python 3 安装包
3. **务必勾选** ✅ **"Add Python to PATH"**
4. 点击 "Install Now"

```powershell
# 验证
python --version
pip --version
```

> 如果不小心没勾选 PATH，可以手动把 `C:\Users\<用户名>\AppData\Local\Programs\Python\Python3xx\` 和 `Scripts\` 目录加到系统环境变量。

### 方式二：winget

```powershell
winget install Python.Python.3
winget install Python.Python.3.12
```

### Windows 注意事项

- Windows 上 `python` 和 `python3` **都可用**（如果只装了一个版本）。
- 长路径问题：安装后可在 PowerShell 中启用长路径支持：
  ```powershell
  New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
    -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
  ```

---

## Linux

### Debian / Ubuntu

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv -y

# 验证
python3 --version
pip3 --version
```

**安装特定版本（deadsnakes PPA）**：

```bash
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update
sudo apt install python3.12 python3.12-venv python3.12-distutils -y
```

### RHEL / Fedora

```bash
# Fedora
sudo dnf install python3 python3-pip -y

# RHEL / CentOS 需先启用 EPEL
sudo dnf install epel-release -y
sudo dnf install python3 python3-pip -y
```

### Arch Linux

```bash
sudo pacman -S python python-pip
```

---

## 平台差异关键点

| 操作 | macOS | Windows | Linux |
|------|-------|---------|-------|
| 命令名 | `python3` / `pip3` | `python` / `pip` | `python3` / `pip3` |
| 安装方式 | brew / pkg | exe安装包 / winget | apt / dnf / pacman |
| 系统自带 | Python 2（不要动） | 无 | 可能有 Python 2 |
| venv 模块 | 需 `python3 -m venv` | 需 `python -m venv` | 需 `sudo apt install python3-venv` |

> **跨平台通用原则**：用 `python3` 和 `pip3` 在 macOS/Linux 上最安全；Windows 上用 `python` 和 `pip`。写脚本时推荐用 `python3`。
