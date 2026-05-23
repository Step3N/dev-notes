# 🪟 新 Windows 配置清单

> 从开箱到开工，逐项打勾 ✅

---

## 1. 系统设置

- [ ] **启用开发者模式** — 允许本地安装应用、打开 SSH 服务、启用设备门户
  ```powershell
  # 设置 → 隐私和安全性 → 开发者选项 → 启用"开发人员模式"
  # 或在 PowerShell (管理员) 中:
  reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /v AllowDevelopmentWithoutDevLicense /t REG_DWORD /d 1 /f
  ```
- [ ] **显示文件扩展名** — 避免被伪装的恶意文件欺骗，开发时也方便区分类型
  ```powershell
  # 打开文件资源管理器 → 查看 → 勾选"文件扩展名"
  ```
- [ ] **显示隐藏文件**
  ```powershell
  # 文件资源管理器 → 查看 → 勾选"隐藏的项目"
  ```
- [ ] **启用长路径支持** — npm 等工具依赖深路径，默认 Windows 路径限制为 260 字符
  ```powershell
  # PowerShell (管理员)
  New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
  ```
- [ ] **关闭快速启动** — 避免双系统时间错误，以及关机不完全导致的问题
  ```powershell
  # PowerShell (管理员)
  powercfg /h off
  ```
- [ ] **设置时区自动同步**
  ```powershell
  # 设置 → 时间和语言 → 自动设置时区
  ```

## 2. 包管理器

> 推荐使用 WinGet（Windows 11 内置） + Scoop（CLI 工具）

- [ ] **安装 WinGet** (Windows 10 需手动) — 微软官方包管理器，系统级 GUI 应用管理
  ```powershell
  # Windows 11 已内置，Windows 10 从 Microsoft Store 安装 "App Installer"
  winget --version
  ```
- [ ] **安装 Scoop** — 命令行工具包管理器，无需管理员权限，软件装在用户目录
  ```powershell
  Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
  irm get.scoop.sh | iex
  ```
- [ ] **添加 Scoop 常用 Bucket**
  ```powershell
  scoop bucket add extras
  scoop bucket add versions
  scoop bucket add nerd-fonts
  ```
- [ ] **安装 Chocolatey** (可选) — 老牌包管理器，社区包最全
  ```powershell
  Set-ExecutionPolicy Bypass -Scope Process -Force
  [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
  iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
  ```

## 3. 终端

- [ ] **安装 Windows Terminal** — 微软新一代终端，支持多标签、GPU 渲染、自定义主题
  ```powershell
  winget install --id Microsoft.WindowsTerminal -e
  ```
- [ ] **安装 PowerShell 7** — 比 Windows 自带 PowerShell 5 更新更快，支持更多特性
  ```powershell
  winget install --id Microsoft.PowerShell -e
  ```
- [ ] **设置 Windows Terminal 默认配置文件为 PowerShell 7**
  ```powershell
  # 打开 Windows Terminal → 设置 → 启动 → 默认配置文件 → 选择 "PowerShell 7"
  ```
- [ ] **安装 Nerd Font** — 终端图标符号支持（如 Oh My Posh 需要）
  ```powershell
  # 在 Windows Terminal 设置中，将 Shell 的字体改为 "CaskaydiaCove Nerd Font" 或 "MesloLGM Nerd Font"
  scoop install Meslo-NF
  # 或从 https://www.nerdfonts.com/ 下载安装
  ```
- [ ] **安装 Oh My Posh** — PowerShell 主题引擎，美观的提示符
  ```powershell
  winget install JanDeDobbeleer.OhMyPosh -e
  ```
- [ ] **配置 PowerShell 配置文件**
  ```powershell
  # 编辑 $PROFILE
  if (!(Test-Path -Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force }
  notepad $PROFILE
  # 添加:
  # oh-my-posh init pwsh | Invoke-Expression
  # Import-Module PSReadLine
  # Set-PSReadLineOption -PredictionSource History
  ```
- [ ] **安装 cli 工具 (Scoop)**
  ```powershell
  scoop install bat ripgrep fd fzf jq gcc make curl wget unzip 7zip
  ```

## 4. WSL2

- [ ] **启用 WSL** — 在 Windows 上运行 Linux 内核，原生开发环境
  ```powershell
  # PowerShell (管理员)
  dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
  ```
- [ ] **启用虚拟机平台** — WSL2 需要
  ```powershell
  # PowerShell (管理员)
  dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
  ```
- [ ] **重启电脑**
- [ ] **设置 WSL2 为默认版本**
  ```powershell
  wsl --set-default-version 2
  ```
- [ ] **安装 Ubuntu 24.04 LTS**
  ```powershell
  winget install --id Canonical.Ubuntu.2404 -e
  # 或从 Microsoft Store 安装 "Ubuntu 24.04 LTS"
  ```
- [ ] **卸载并升级 WSL2 内核** (如果需要)
  ```powershell
  wsl --update
  ```
- [ ] **初始化 Ubuntu** — 设置用户名和密码
  ```powershell
  wsl --set-default Ubuntu-24.04
  wsl
  # 在 WSL 内设置:
  sudo apt update && sudo apt upgrade -y
  ```
- [ ] **验证 WSL2**
  ```powershell
  wsl -l -v
  # 确认 Ubuntu 为 WSL2 版本
  ```

## 5. Git

- [ ] **安装 Git**
  ```powershell
  winget install --id Git.Git -e
  ```
- [ ] **配置 Git**
  ```powershell
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  git config --global init.defaultBranch main
  git config --global core.autocrlf input  # WSL 兼容
  ```
- [ ] **生成 SSH Key**
  ```powershell
  ssh-keygen -t ed25519 -C "your.email@example.com"
  Get-Content ~\.ssh\id_ed25519.pub | Set-Clipboard
  Write-Host "公钥已复制到剪贴板，请添加到 GitHub SSH Keys"
  ```
- [ ] **配置 SSH Agent**
  ```powershell
  Get-Service ssh-agent | Set-Service -StartupType Automatic
  Start-Service ssh-agent
  ssh-add ~\.ssh\id_ed25519
  ```
- [ ] **验证 SSH 连接**
  ```powershell
  ssh -T git@github.com
  ```

## 6. 编程语言环境

- [ ] **安装 pyenv-win** — Windows 上的 Python 版本管理
  ```powershell
  # 用 Scoop 安装
  scoop install pyenv
  pyenv install 3.12
  pyenv global 3.12
  python --version
  ```
- [ ] **安装 nvm-windows** — Windows 上的 Node.js 版本管理
  ```powershell
  winget install --id CoreyButler.NVMforWindows -e
  # 重启终端后
  nvm install lts
  nvm use lts
  node --version && npm --version
  ```
- [ ] **安装 pnpm**
  ```powershell
  npm install -g pnpm
  ```
- [ ] **安装 Rust** (可选)
  ```powershell
  # 下载 https://rustup.rs/ 或:
  winget install --id Rustlang.Rustup -e
  rustup default stable
  rustc --version
  ```
- [ ] **安装 Go** (可选)
  ```powershell
  winget install --id GoLang.Go -e
  go version
  ```

## 7. Docker Desktop with WSL2

- [ ] **安装 Docker Desktop** — 使用 WSL2 后端，性能接近原生 Linux
  ```powershell
  winget install --id Docker.DockerDesktop -e
  ```
- [ ] **配置 WSL2 后端** — Docker Desktop → Settings → Resources → WSL Integration → 启用 Ubuntu
- [ ] **验证 Docker**
  ```powershell
  docker --version
  docker run hello-world
  ```

## 8. VS Code

- [ ] **安装 VS Code**
  ```powershell
  winget install --id Microsoft.VisualStudioCode -e
  ```
- [ ] **安装 VS Code Insiders** (可选，尝鲜版)
  ```powershell
  winget install --id Microsoft.VisualStudioCode.Insiders -e
  ```
- [ ] **安装 WSL 扩展** — 让 VS Code 直接在 WSL 内编辑代码
  ```powershell
  code --install-extension ms-vscode-remote.remote-wsl
  ```
- [ ] **登录同步设置**
  ```powershell
  # VS Code → 设置 → 打开 Settings Sync → 登录 GitHub
  ```

## 9. 浏览器

- [ ] **安装 Chrome**
  ```powershell
  winget install --id Google.Chrome -e
  ```
- [ ] **安装 Firefox** (可选)
  ```powershell
  winget install --id Mozilla.Firefox -e
  ```

## 10. 常用应用

- [ ] **安装日常应用**
  ```powershell
  winget install --id Microsoft.PowerToys -e       # 微软效率工具（FancyZones 窗口管理、颜色选取等）
  winget install --id Obsidian.Obsidian -e          # 笔记工具
  winget install --id 7zip.7zip -e                  # 压缩解压
  winget install --id VideoLAN.VLC -e               # 视频播放器
  winget install --id Notion.Notion -e              # 项目管理/笔记
  winget install --id AgileBits.1Password -e        # 密码管理器
  winget install --id Discord.Discord -e            # 团队沟通
  winget install --id Spotify.Spotify -e            # 音乐
  winget install --id SlackTechnologies.Slack -e    # 工作沟通
  winget install --id Microsoft.Office -e           # Office 套件
  ```

## 11. WSL 内额外配置

> 在 WSL (Ubuntu) 内执行以下操作:

- [ ] **配置 .zshrc 别名**
  ```bash
  echo 'alias ll="ls -lah"' >> ~/.zshrc
  echo 'alias gs="git status"' >> ~/.zshrc
  ```
- [ ] **配置 Git 全局忽略大小写** (WSL 默认区分大小写)
  ```bash
  git config --global core.ignorecase false
  ```
- [ ] **WSL 内存限制** (避免 Docker 占满宿主机内存)
  ```powershell
  # 在 Windows 用户目录创建 .wslconfig
  notepad "$env:USERPROFILE\.wslconfig"
  ```
  写入:
  ```
  [wsl2]
  memory=8GB
  processors=4
  swap=2GB
  ```
- [ ] **重启 WSL**
  ```powershell
  wsl --shutdown
  wsl
  ```

## 12. 验证清单

- [ ] **检查所有工具版本**
  ```powershell
  Write-Host "=== 开发环境验证 ==="
  git --version
  python --version
  node --version
  npm --version
  docker --version
  code --version
  wsl -l -v
  Write-Host "=== 全部就绪 ✅ ==="
  ```

---

> ✅ 全部完成？恭喜，你的 Windows 已经准备好开工了！
