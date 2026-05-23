# iTerm2 — macOS 最佳终端

> **平台**：仅 macOS

## 安装

```bash
# Homebrew（推荐）
brew install --cask iterm2

# 或官网直接下载
# https://iterm2.com/downloads.html
```

## 为什么用 iTerm2？

macOS 自带 Terminal.app 功能简陋。iTerm2 提供：

| 特性 | iTerm2 | Terminal.app |
|------|--------|--------------|
| 分屏 | ✅ | ❌ |
| 热键窗口 | ✅ | ❌ |
| 搜索 | ✅ | ❌ |
| 粘贴历史 | ✅ | ❌ |
| 触发器 | ✅ | ❌ |
| 配置可迁移 | ✅ | ❌ |

## 核心功能

### 分屏 (Split Panes)

| 快捷键 | 操作 |
|--------|------|
| `⌘D` | 垂直分屏（左右） |
| `⌘⇧D` | 水平分屏（上下） |
| `⌘⌥I` | 所有分屏同步输入 |
| `⌘]` / `⌘[` | 切换分屏 |

### 热键窗口 (Hotkey Window)

**个人最爱功能** — 按 `⌘\`` 从任意界面弹出/隐藏终端。

**配置**：Settings → Keys → Hotkey → 勾选 "Show/hide all windows with a system-wide hotkey"

### 搜索

| 快捷键 | 操作 |
|--------|------|
| `⌘F` | 打开搜索 |
| `Tab` | 补全选中的搜索词 |
| `⌘G` / `⌘⇧G` | 下一个/上一个匹配 |

### 粘贴历史

`⌘⇧H` — 弹出粘贴历史面板，记录你复制过的所有内容。

### Instant Replay

`⌘⌥B` — 回滚终端输出（比如编译日志滚过去了，可以往回翻）。

### 触发器 (Triggers)

Settings → Profiles → Advanced → Triggers

自动高亮关键词（如：ERROR 显示红底白字，WARNING 显示黄底）。

**示例正则**：

| 正则 | 动作 | 颜色 |
|------|------|------|
| `.*ERROR.*` | Highlight | 红底 |
| `.*WARNING.*` | Highlight | 黄底 |
| `.*SUCCESS.*` | Highlight | 绿底 |

### Profiles — 核心配置

Settings → Profiles → 创建/编辑 Profile

| 配置项 | 推荐值 |
|--------|--------|
| 字体 | JetBrains Mono / FiraCode Nerd Font |
| 字号 | 14 |
| 配色方案 | Dracula / One Dark / Solarized |
| 背景透明度 | 0.85（毛玻璃效果） |
| Working Directory | Reuse previous session's directory |

### 自然文本编辑

在 Profiles → Keys 中设置 **Left/Right Option Key 为 Esc+**，则可：

| 快捷键 | 操作 |
|--------|------|
| `⌥←` | 按单词左跳 |
| `⌥→` | 按单词右跳 |
| `⌥⌫` | 删除前一个单词 |
| `⌥D` | 删除后一个单词 |

## 推荐配置

### 字体 — JetBrains Mono

```bash
brew install --cask font-jetbrains-mono-nerd-font
```

### 配色方案 — Dracula

```bash
# 下载 Dracula 配色
git clone https://github.com/dracula/iterm2.git /tmp/dracula-iterm2
# 在 iTerm2 中导入：Settings → Profiles → Colors → Color Presets → Import
# 选择 /tmp/dracula-iterm2/Dracula.itermcolors
```

### 配色方案 — One Dark

```bash
# 搜索 "One Dark iTerm2" 下载 .itermcolors 文件
# 或使用 https://github.com/nicknisi/one-dark-iterm2
```

## 与 tmux 集成

iTerm2 内置 tmux 集成：

```
# 在 iTerm2 中启用 tmux 集成
# 它会把 tmux 的 window/pane 映射为 iTerm2 的原生 tab/split
```

- AppleScript 支持
- Python API 自动化
- 自动保存/恢复窗口布局（Settings → General → Startup → "Use System Window Restoration"）
- 验证安装：
  ```bash
  iterm2 --version  # 如果是 brew 安装
  ```
  或打开 iTerm2 → About iTerm2

---

**参考**：https://iterm2.com/documentation.html
