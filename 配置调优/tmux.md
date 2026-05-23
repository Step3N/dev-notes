# Tmux — 终端复用器

> **平台**：macOS / Linux（WSL 可用）
> **Windows**：无原生支持，需 WSL

## 安装

```bash
# macOS
brew install tmux

# Debian/Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux

# Arch
sudo pacman -S tmux

# Windows (WSL)
# 在 WSL Linux 发行版中使用 apt/dnf/pacman 安装
```

### 验证

```bash
tmux -V
# 输出示例：tmux 3.4
```

## 核心概念

```
tmux
├── Sessions（会话）— 保存整个工作环境，可 detach/re-attach
│   ├── Windows（窗口）— 类似 Tab
│   │   ├── Panes（窗格）— 分割的面板
│   │   └── Panes
│   └── Windows
└── Sessions
```

## 外部命令（不在 tmux 内运行）

```bash
# 创建新会话（命名）
tmux new -s myproject

# 创建不附加的新会话
tmux new -s myproject -d

# 附加到已有会话
tmux attach -t myproject

# 列出所有会话
tmux ls

# 关闭会话
tmux kill-session -t myproject

# 关闭所有会话
tmux kill-server

# 重命名会话
tmux rename-session -t oldname newname
```

## 内部快捷键（在 tmux 内运行）

默认前缀键：`Ctrl+b`（下文用 `Prefix` 表示）

### 窗格 (Panes)

| 快捷键 | 操作 |
|--------|------|
| `Prefix "` | 水平分割（上下） |
| `Prefix %` | 垂直分割（左右） |
| `Prefix 方向键` | 切换窗格 |
| `Prefix {` | 当前窗格左移 |
| `Prefix }` | 当前窗格右移 |
| `Prefix z` | 全屏/还原当前窗格 |
| `Prefix !` | 将窗格弹出为新窗口 |
| `Prefix x` | 关闭当前窗格（需确认） |
| `Prefix q` | 显示窗格编号（按编号跳转） |
| `Prefix Ctrl+方向键` | 调整窗格大小（1 格） |
| `Prefix Alt+方向键` | 调整窗格大小（5 格） |
| `Prefix Space` | 切换布局 |
| `Prefix M-1~5` | 预设布局 |

### 窗口 (Windows)

| 快捷键 | 操作 |
|--------|------|
| `Prefix c` | 创建新窗口 |
| `Prefix ,` | 重命名当前窗口 |
| `Prefix w` | 窗口列表（交互选择） |
| `Prefix n` | 下一个窗口 |
| `Prefix p` | 上一个窗口 |
| `Prefix 数字` | 跳转到指定窗口 |
| `Prefix &` | 关闭当前窗口（需确认） |
| `Prefix .` | 移动窗口（按编号） |
| `Prefix f` | 按名称搜索窗口 |

### 会话 (Sessions)

| 快捷键 | 操作 |
|--------|------|
| `Prefix d` | 分离当前会话（回到普通终端） |
| `Prefix s` | 交互式会话/窗口选择器 |
| `Prefix (` | 上一个会话 |
| `Prefix )` | 下一个会话 |
| `Prefix L` | 切换到上一个会话 |
| `Prefix $` | 重命名会话 |

### 复制模式 (Copy Mode)

| 快捷键 | 操作 |
|--------|------|
| `Prefix [` | 进入复制模式 |
| `PgUp` / `PgDn` | 翻页 |
| `方向键` | 移动光标 |
| `Space` | 开始选择 |
| `Enter` | 复制选中文本 |
| `Prefix ]` | 粘贴复制的内容 |
| `q` | 退出复制模式 |

### 其他

| 快捷键 | 操作 |
|--------|------|
| `Prefix t` | 显示时间（大时钟） |
| `Prefix ?` | 显示所有快捷键 |
| `Prefix :` | 进入 tmux 命令行 |
| `Prefix ~` | 显示日志（调试用） |
| `Prefix r` | 重新加载配置 |

## 配置：`~/.tmux.conf`

### 推荐配置

```tmux
# 设置前缀键为 Ctrl+a（更顺手，替代 Ctrl+b）
set -g prefix C-a
unbind C-b
bind C-a send-prefix

# 启用鼠标（滚轮、点击切换、调整大小）
set -g mouse on

# 256 色支持
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",xterm-256color:Tc"

# 设置窗口编号从 1 开始
set -g base-index 1
setw -g pane-base-index 1

# 重新加载配置的快捷键（Prefix r）
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"

# 使用 vi 风格的复制模式
setw -g mode-keys vi

# 窗格分割快捷键更直观（| 和 -）
bind | split-window -h
bind - split-window -v

# 更快的窗格切换（Alt+方向键）
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# 调整窗口大小（Prefix H/J/K/L）
bind H resize-pane -L 5
bind J resize-pane -D 5
bind K resize-pane -U 5
bind L resize-pane -R 5

# 状态栏样式
set -g status-style "fg=#ffffff,bg=#2d2d2d"
set -g status-left "#[fg=#ffffff,bg=#e06c75] #S "
set -g status-right "#[fg=#ffffff,bg=#2d2d2d] %Y-%m-%d %H:%M "
setw -g window-status-current-style "fg=#ffffff,bg=#61afef"
```

加载配置：

```bash
tmux source-file ~/.tmux.conf
```

## Tmux Plugin Manager (TPM)

```bash
# 安装 TPM
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

配置 `~/.tmux.conf`：

```tmux
# TPM 配置（放在文件末尾）
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

run '~/.tmux/plugins/tpm/tpm'
```

```bash
# 安装插件：Prefix + I（大写 I）
# 更新插件：Prefix + U
# 移除插件：删除配置行，Prefix + Alt+u
```

### 推荐插件

| 插件 | 功能 |
|------|------|
| `tmux-sensible` | 实用默认配置集 |
| `tmux-resurrect` | 保存/恢复 tmux 环境（Prefix Ctrl+s 保存，Prefix Ctrl+r 恢复） |
| `tmux-continuum` | 自动保存，重启后恢复 |
| `tmux-yank` | 系统剪贴板复制（Prefix y） |
| `tmux-copycat` | 增强搜索（文件路径、git 状态、正则） |
| `tmux-finger` | 高亮并复制 IP、日期等模式 |

## 实用工作流

### 临时离开，回来继续

```bash
# 在 tmux 中
Prefix d           # detached，回到普通终端

# 回家 / 关笔记本
# 第二天到公司
tmux attach        # 全部恢复！
```

### 项目会话模式

```bash
# 每个项目一个会话
tmux new -s backend
tmux new -s frontend
tmux new -s docs

# 切换
tmux attach -t backend
```

### .tmuxinator 或脚本自动化

```bash
# tmuxp — Python 工具，YAML 配置 tmux 布局
pip install tmuxp
```

## 与 iTerm2 集成

iTerm2 内置 tmux 集成（`tmux -CC` 模式），将 tmux 的 windows/panes 映射为 iTerm2 的 tabs/splits。

---

**参考**：
- https://github.com/tmux/tmux/wiki
- https://github.com/tmux-plugins/tpm
- https://tmuxcheatsheet.com/
