# Git 安装

跨平台安装 Git 并验证。

---

## macOS

### 方式一：Homebrew（推荐）

```bash
brew install git
```

### 方式二：Xcode Command Line Tools（自带 git）

```bash
xcode-select --install
```

验证：

```bash
git --version
# 输出示例：git version 2.45.0
```

### 配置换行符

```bash
git config --global core.autocrlf input
```

macOS 用 `input` —— 提交时转 LF，检出时不转换。

---

## Windows

### 方式一：winget（推荐，Windows 10 1709+）

```powershell
winget install --id Git.Git -e --source winget
```

### 方式二：官方安装程序

下载 <https://git-scm.com/download/win>，一路默认即可，注意这步：

- 在 "Configuring the line ending conversions" 页面选择 **Checkout as-is, commit Unix-style line endings**

### 方式三：Git for Windows（传统）

```powershell
winget install --id GitForWindows.Git -e
```

验证（打开新的 PowerShell / CMD）：

```powershell
git --version
# git version 2.45.0.windows.1
```

### 配置换行符

```powershell
git config --global core.autocrlf true
```

Windows 用 `true` —— 检出时转 CRLF，提交时转 LF。

---

## Linux

### Debian / Ubuntu（apt）

```bash
sudo apt update && sudo apt install git -y
```

### Fedora / RHEL（dnf）

```bash
sudo dnf install git -y
```

### Arch Linux（pacman）

```bash
sudo pacman -S git --noconfirm
```

验证：

```bash
git --version
# git version 2.43.0
```

### 配置换行符

```bash
git config --global core.autocrlf input
```

Linux 用 `input`，理由同 macOS。

---

## 升级 Git

| 平台 | 命令 |
|------|------|
| **macOS** (brew) | `brew upgrade git` |
| **Windows** (winget) | `winget upgrade --id Git.Git` |
| **Linux** (apt) | `sudo apt update && sudo apt upgrade git -y` |
| **Linux** (dnf) | `sudo dnf upgrade git -y` |
| **Linux** (pacman) | `sudo pacman -Syu git --noconfirm` |

---

## 验证完整安装

```bash
git --version && git config --global core.autocrlf
```

确认输出版本号和 autocrlf 配置值。
