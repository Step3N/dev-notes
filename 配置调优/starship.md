# Starship — 跨平台极简提示符

> **平台**：全平台（macOS / Linux / Windows）

## 安装

### macOS

```bash
brew install starship
```

### Windows

```powershell
winget install starship
```

### Linux

```bash
# curl 一键安装
curl -sS https://starship.rs/install.sh | sh

# 包管理器
sudo apt install starship      # Debian/Ubuntu (可能需要添加仓库)
sudo dnf install starship      # Fedora
sudo pacman -S starship        # Arch
```

### 验证安装

```bash
starship --version
```

## Shell 初始化

### Zsh（macOS/Linux）

```bash
# 添加到 ~/.zshrc 末尾
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
source ~/.zshrc
```

### PowerShell（Windows/macOS/Linux）

```powershell
# 添加到 $PROFILE
echo "invoke-expression (&starship init powershell)" >> $PROFILE
. $PROFILE
```

### Bash（Linux/WSL）

```bash
echo 'eval "$(starship init bash)"' >> ~/.bashrc
source ~/.bashrc
```

### Fish

```bash
echo 'starship init fish | source' >> ~/.config/fish/config.fish
```

### 验证生效

```bash
# 应输出 starship 默认提示符
exec $SHELL
```

## 配置

配置文件路径：`~/.config/starship.toml`

```toml
# 一个最简配置示例
format = """
[](#9A348E)\
$os\
$directory\
$git_branch\
$git_status\
[](#9A348E)\
$nodejs\
$python\
$rust\
$line_break\
$character"""

[os]
disabled = false

[directory]
truncation_length = 3

[git_branch]
format = " [$branch]($style) "
style = "bold purple"

[git_status]
conflicted = "🏳"
ahead = "⇡${count}"
behind = "⇣${count}"
diverged = "⇕⇡${ahead_count}⇣${behind_count}"

[nodejs]
format = " [$symbol($version )]($style)"
style = "bold green"

[python]
format = " [$symbol($version )]($style)"
style = "bold yellow"

[rust]
format = " [$symbol($version )]($style)"
style = "bold red"

[character]
success_symbol = "[➜](bold green)"
error_symbol = "[➜](bold red)"
```

## Modules（模块）

Starship 的模块自动检测，仅在需要时显示：

| 模块 | 显示条件 | 示例 |
|------|----------|------|
| `directory` | 总是 | `~/projects/my-app` |
| `git_branch` | Git 仓库 | `main` |
| `git_status` | Git 仓库有变更 | `⇡1 +2 ~3` |
| `nodejs` | 有 `.nvmrc`/`package.json`/`node_modules` | `⬡ v20.10.0` |
| `python` | 有 `.python-version`/`Pipfile`/`pyproject.toml` | `🐍 3.12.0` |
| `rust` | 有 `Cargo.toml` | `🦀 1.75.0` |
| `go` | 有 `go.mod` | `🐹 1.21.0` |
| `docker_context` | Docker 上下文非默认 | `🐳 → production` |
| `command_duration` | 上条命令耗时超阈值 | `12ms` |
| `line_break` | 换行 | — |
| `os` | 显示系统图标（需 Nerd Font） |  |
| `shell` | 显示当前 Shell 名称 | `zsh` |
| `hostname` | SSH 连接中 | `server01` |
| `kubernetes` | 有 kubeconfig 上下文 | `☸ → prod` |
| `package` | 版本管理文件 | `v1.2.3` |
| `time` | 可选，当前时间 | `15:30` |
| `username` | 仅 root/ssh | `root` |

### 常用模块配置片段

```toml
# 目录 — 最多 3 级，显示 README 符号
[directory]
truncation_length = 3
truncate_to_repo = true
read_only = " 🔒"

# Git 分支 — 完整分支名
[git_branch]
truncation_length = 0  # 不截断

# 命令耗时 — 超过 5 秒显示
[command_duration]
min_time = 5000
show_milliseconds = false

# Docker 上下文
[docker_context]
format = " [$symbol$context]($style) "

# Python — 显示虚拟环境名
[python]
python_binary = ["python3", "python"]
detect_extensions = ["py"]
```

## Presets（预设）

```bash
# 查看所有预设
starship preset list

# 预览预设效果
starship preset nerd-font-symbols

# 应用预设（覆盖 ~/.config/starship.toml）
starship preset nerd-font-symbols -o ~/.config/starship.toml
```

### 推荐预设

| 预设名 | 特点 |
|--------|------|
| `nerd-font-symbols` | Nerd Font 图标版本 |
| `no-nerd-font` | 纯文本，无需额外字体 |
| `bracketed-segments` | 用方括号分隔模块 |
| `pastel-powerline` | 粉彩风格，经典 Powerline |
| `tokyo-night` | Tokyo Night 配色 |

## 故障排查

```bash
# 打印完整的调试信息
starship explain

# 打印当前配置
starship config --list

# 重置为默认配置
starship config --reset

# 检查 TOML 语法
starship timings  # 显示各模块耗时
```

## Starship vs Oh My Posh

| 对比项 | Starship | Oh My Posh |
|--------|----------|------------|
| 语言 | Rust | Go |
| 速度 | 极快（< 1ms） | 快 |
| Shell 支持 | zsh/bash/fish/pwsh/... | 仅 pwsh |
| 配置格式 | TOML | JSON |
| 主题预设 | 一键切换 | Get-PoshThemes |
| 无需 Nerd Font | 有 no-nerd-font 预设 | 必需 |
| 自动检测模块 | ✅ | 需手动配置 |

---

**参考**：https://starship.rs/guide/
