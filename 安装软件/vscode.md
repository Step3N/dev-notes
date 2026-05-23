# VS Code 安装

## macOS

### Homebrew（推荐）

```bash
brew install --cask visual-studio-code
```

### 官网下载

<https://code.visualstudio.com/Download> — 下载 macOS 版（Universal / Apple Silicon / Intel）。

### 安装 `code` 命令（PATH）

打开 VS Code → `⌘⇧P` → 输入 `Shell Command: Install 'code' command in PATH`。

验证：

```bash
code --version
```

---

## Windows

### winget（推荐）

```powershell
winget install Microsoft.VisualStudioCode
```

或带额外参数：

```powershell
winget install --id Microsoft.VisualStudioCode --source winget
```

### 官网下载

<https://code.visualstudio.com/Download> — 下载 Windows 版（User Installer / System Installer）。

> User Installer 不需要管理员权限；System Installer 需要管理员权限且为所有用户安装。

### Scoop

```powershell
scoop bucket add extras
scoop install vscode
```

### Chocolatey

```powershell
choco install vscode
```

### `code` 命令

Windows 版安装后默认已将 `code` 加入 PATH。若未生效，重启终端或手动将以下路径加入 PATH：

```
C:\Users\<用户名>\AppData\Local\Programs\Microsoft VS Code\bin
```

验证：

```powershell
code --version
```

---

## Linux

### 通过 Microsoft 仓库安装 .deb（Debian / Ubuntu）

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt update
sudo apt install code
```

### .rpm（Fedora / RHEL）

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
dnf check-update
sudo dnf install code
```

### Snap

```bash
sudo snap install code --classic
```

### `code` 命令

上述安装方式默认已将 `code` 加入 PATH。验证：

```bash
code --version
```

---

## 启动项目

```bash
code .
code /path/to/project
```

---

## 配置同步

- **内置 Settings Sync**：登录 GitHub/Microsoft 账号后，齿轮图标 → 「Turn on Settings Sync」
- **Settings.json + Git**：手动将 `~/.config/Code/User/settings.json` 等文件纳入 Git 管理（详见 `vscode-配置同步.md`）

---

## 平台差异速查

| 项目 | macOS | Windows | Linux |
|------|-------|---------|-------|
| 配置目录 | `~/Library/Application Support/Code/User/` | `%APPDATA%\Code\User\` | `~/.config/Code/User/` |
| 扩展目录 | `~/.vscode/extensions` | `%USERPROFILE%\.vscode\extensions` | `~/.vscode/extensions` |
| 安装命令 | `brew install --cask` | `winget install` | `apt install code` |
| PATH 安装 | 需手动执行 Shell Command | 自动加入 | 自动加入 |
