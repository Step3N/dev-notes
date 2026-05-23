# Linux 终端美化

> 让你的 Linux 终端告别黑白，变得美观易用

**适用平台**：Linux
**前置条件**：
- 已安装终端模拟器（GNOME Terminal / Konsole / Kitty 等）
- 有基础 Shell 使用经验

## 字体安装

推荐使用 Nerd Fonts（带图标），常用选择：

```bash
# Debian/Ubuntu
sudo apt install fonts-firacode fonts-noto-color-emoji

# Fedora
sudo dnf install fira-code-fonts

# Arch
sudo pacman -S ttf-fira-code
```

**手动安装 Nerd Font**：
```bash
# 下载 JetBrainsMono Nerd Font
wget https://github.com/ryanoasis/nerd-fonts/releases/latest/download/JetBrainsMono.zip
unzip JetBrainsMono.zip -d ~/.local/share/fonts/
fc-cache -fv
```

下载后需在终端模拟器的设置中选择该字体。

## 终端模拟器推荐

| 终端 | 特点 | 安装 |
|------|------|------|
| Kitty | GPU 加速，分屏支持好 | `apt install kitty` |
| Alacritty | 极简，GPU 加速，YAML 配置 | `apt install alacritty` |
| GNOME Terminal | Ubuntu 默认，简单 | 通常预装 |
| Konsole | KDE 默认，功能丰富 | `apt install konsole` |
| tmux | 终端复用器（非模拟器） | `apt install tmux` |

以 **Kitty** 为例的基础配置（`~/.config/kitty/kitty.conf`）：
```conf
font_family JetBrainsMono Nerd Font
font_size 13
foreground #cdd6f4
background #1e1e2e
selection_foreground #1e1e2e
selection_background #f5e0dc
```

## Shell 配置美化

### 1. Zsh + Oh My Zsh

安装 Zsh 和 Oh My Zsh：
```bash
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

推荐使用 **Powerlevel10k** 主题：
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

在 `~/.zshrc` 中设置：
```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

运行 `p10k configure` 进入交互式配置向导。

### 2. Starship（跨 Shell）

安装：
```bash
curl -sS https://starship.rs/install.sh | sh
```

在 `~/.zshrc` / `~/.bashrc` 末尾添加：
```bash
eval "$(starship init zsh)"
```

配置 `~/.config/starship.toml`：
```toml
[nodejs]
symbol = "🟢 "
format = "via [$symbol($version)]($style) "

[git_branch]
symbol = "🌱 "

[character]
success_symbol = "[➜](bold green) "
error_symbol = "[➜](bold red) "
```

### 3. Bash 基础美化

如果你不想换 Zsh，可以给 Bash 加点颜色：

编辑 `~/.bashrc`：
```bash
# 彩色提示符
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
# 启用 ls 颜色
alias ls='ls --color=auto'
alias grep='grep --color=auto'
```

## 颜色方案

热门配色方案：

| 方案 | 特点 | 链接 |
|------|------|------|
| Catppuccin | 柔和配色，三款口味 | github.com/catppuccin |
| Dracula | 深色经典 | github.com/dracula |
| Nord | 北极蓝调 | github.com/arcticicestudio/nord |
| Tokyo Night | 霓虹风格 | github.com/enkia/tokyo-night-vscode-terminal |

以 **Catppuccin Mocha** 为例（Kitty）：
```conf
# Catppuccin Mocha
foreground #cdd6f4
background #1e1e2e
color0 #45475a
color1 #f38ba8
color2 #a6e3a1
color3 #f9e2af
color4 #89b4fa
color5 #f5c2e7
color6 #94e2d5
color7 #bac2de
color8 #585b70
color9 #f38ba8
color10 #a6e3a1
color11 #f9e2af
color12 #89b4fa
color13 #f5c2e7
color14 #94e2d5
color15 #a6adc8
```

## 插件增强

### Zsh 插件
```bash
# 在 ~/.zshrc 中
plugins=(git zsh-autosuggestions zsh-syntax-highlighting web-search docker)
```

安装额外插件：
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### fzf（模糊搜索）
```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

### zoxide（智能 cd）
```bash
curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh
# 添加到 shell 配置
eval "$(zoxide init zsh)"
```

## 验证美化效果

重启终端后检查：
- 字体显示图标（Nerd Fonts 生效）
- 提示符显示 Git 分支信息
- ls 输出有颜色
- 输入命令时有语法高亮

## 常见问题

| 问题 | 解决方案 |
|------|----------|
| 图标显示为方框 | 确认已安装 Nerd Fonts 并在终端设置中选中 |
| Powerlevel10k 显示乱码 | 运行 `p10k configure` 重新配置，或检查字体 |
| Oh My Zsh 启动慢 | 精简插件列表，禁用不需要的插件 |
| Starship 不显示 | `echo $?` 检查 init 命令是否在 shell 配置中 |

📝 **更新记录**
| 日期 | 更新内容 |
|------|----------|
| 2026-05-23 | 初始版本 |
