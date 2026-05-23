# Linux 开发者必备软件清单

> 以下默认基于 Ubuntu/Debian（apt），Fedora（dnf）和 Arch（pacman）的命令附在括号内。

## 开发工具

| 软件 | 说明 | 安装（Ubuntu / Fedora / Arch） |
|------|------|--------------------------------|
| **VS Code** | 代码编辑器标配，Remote SSH 开发首选 | `sudo snap install code` 或添加 apt 仓库后 `sudo apt install code`；Fedora: `sudo rpm --import ...` 添加 repo；Arch: `yay -S visual-studio-code-bin` |
| **JetBrains Toolbox** | IDE 版本管理器，下载解压即可用 | 官网下载 tar.gz 解压运行 |
| **Docker Engine** | 容器运行时，不含 Desktop GUI | `curl -fsSL get.docker.com \| sh`；Fedora: `sudo dnf install docker-ce`；Arch: `sudo pacman -S docker` |
| **Postman** | API 调试 | `sudo snap install postman` 或 `flatpak install flathub com.getpostman.Postman` |
| **DBeaver** | 数据库 GUI 客户端，支持 20+ 数据库 | `sudo snap install dbeaver-ce`；Fedora: `sudo dnf install dbeaver`；Arch: `yay -S dbeaver` |
| **Insomnia** | 轻量 API 客户端 | `sudo snap install insomnia` 或官网下载 AppImage |
| **Neovim** | 现代 Vim，Lua 配置，LSP 原生支持 | `sudo apt install neovim`；Fedora: `sudo dnf install neovim`；Arch: `sudo pacman -S neovim` |
| **Git** | 版本控制 | `sudo apt install git`；Fedora/Arch 自带或对应包管理器安装 |

## 系统工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **btop** | 现代化资源监视器，鼠标支持，主题丰富 | `sudo apt install btop`；Fedora: `sudo dnf install btop`；Arch: `sudo pacman -S btop` |
| **neofetch** | 终端显示系统信息 + ASCII 发行版 Logo | `sudo apt install neofetch`；Fedora: `sudo dnf install neofetch`；Arch: `sudo pacman -S neofetch` |
| **htop** | 经典进程管理器，树形视图 | `sudo apt install htop` |
| **Timeshift** | 系统快照备份/恢复，类似 Windows 还原点 | `sudo apt install timeshift`；Arch: `yay -S timeshift` |
| **Kitty** | GPU 加速终端模拟器，分屏/远程文件传输 | `sudo apt install kitty`；Fedora: `sudo dnf install kitty`；Arch: `sudo pacman -S kitty` |
| **tmux** | 终端复用器，SSH 会话保持、分屏 | `sudo apt install tmux` |
| **Flameshot** | 截图工具，内置编辑/标注/上传 | `sudo apt install flameshot` |
| **gnome-tweaks** | GNOME 桌面高级设置（字体/扩展/窗口） | `sudo apt install gnome-tweaks` |

## 效率工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **Obsidian** | 本地 Markdown 笔记 | `sudo snap install obsidian` 或官网下载 AppImage |
| **Chrome** | 开发者工具标准浏览器 | 官网下载 .deb 或 `wget -q -O - https://dl.google.com/linux/linux_signing_key.pub \| sudo apt-key add -` 添加源 |
| **Firefox** | 隐私浏览器，Ubuntu 默认 | `sudo apt install firefox` |
| **Bitwarden** | 开源密码管理器 | `sudo snap install bitwarden`；Arch: `yay -S bitwarden-bin` |
| **fcitx5** | Linux 中文输入法框架，支持拼音/五笔 | `sudo apt install fcitx5 fcitx5-chinese-addons`；Arch: `sudo pacman -S fcitx5-im fcitx5-chinese-addons` |
| **Zsh + Oh My Zsh** | 交互式 shell 替代 bash，自动补全/主题 | `sudo apt install zsh && sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **fzf** | 命令行模糊搜索神器，文件/历史/进程通吃 | `sudo apt install fzf`；Arch: `sudo pacman -S fzf` |

## 通讯工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **Slack** | 团队协作 | `sudo snap install slack` 或官网下载 .deb |
| **Telegram Desktop** | 安全即时通讯 | `sudo snap install telegram-desktop`；Fedora: `sudo dnf install telegram-desktop`；Arch: `sudo pacman -S telegram-desktop` |
| **Discord** | 游戏/开发者社群 | `sudo snap install discord` 或官网下载 .deb |
| **WeChat** | 微信（Linux 可用 wine/优麒麟版） | `sudo snap install electronic-wechat`（第三方）或 wine 安装 |

## 建议 Workflow

```bash
# Ubuntu 快速配置
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl wget zsh tmux htop neofetch flameshot
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

> Arch 用户推荐 `yay` 作为 AUR helper。Fedora 用户可启用 RPM Fusion 仓库获取更多软件。
> Linux 开发的优势在于包管理器原生、Docker 无开销、终端体验最佳。
