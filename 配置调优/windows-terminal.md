# Windows Terminal — Windows 现代终端

> **平台**：仅 Windows

## 安装

```powershell
# 方式一：winget（推荐）
winget install Microsoft.WindowsTerminal

# 方式二：Microsoft Store
# 搜索 "Windows Terminal" 并安装

# 方式三：GitHub Releases
# https://github.com/microsoft/terminal/releases
```

## 为什么用 Windows Terminal？

传统 Windows 终端（conhost/cmd）功能简陋。Windows Terminal 是微软新一代终端：

| 特性 | Windows Terminal | conhost |
|------|------------------|---------|
| GPU 加速渲染 | ✅ | ❌ |
| 多 Tab/分屏 | ✅ | ❌ |
| Unicode/Emoji | ✅ | ❌ |
| 多 Profile | ✅ | ❌ |
| 自定义主题 | ✅ | ❌ |
| 可配置快捷键 | ✅ | ❌ |

## 配置方式

### 两种方式

1. **Settings UI**（推荐新手）：`Ctrl+,` 打开图形化设置
2. **settings.json**（推荐高级用户）：在 UI 中点击左下角 "打开 JSON 文件"

### settings.json 位置

```powershell
# 默认位置
%LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json
```

## Profiles

Windows Terminal 支持多种 Shell Profile，可以同时使用：

| Profile | 说明 | 安装命令 |
|---------|------|---------|
| PowerShell | 自带 | 无需安装 |
| Command Prompt | 自带 | 无需安装 |
| WSL | Linux 子系统 | `wsl --install` |
| Azure Cloud Shell | 云端 | 无需安装 |
| Git Bash | Git 命令行 | `winget install Git.Git` |
| Miniconda | Python 环境 | `winget install Anaconda.Miniconda3` |

### 添加 WSL Profile

```powershell
# 安装 WSL 后自动添加
wsl --install -d Ubuntu
```

## 配色方案与主题

### 内置配色

- Campbell（默认）
- One Half Dark / Light
- Solarized Dark / Light
- Tango Dark / Light
- Vintage

### 安装社区主题

```powershell
# 使用 windows-terminal-themes
# https://windowsterminalthemes.dev/
# 复制 JSON 到 settings.json 的 "schemes" 数组
```

## 关键快捷键

| 快捷键 | 操作 |
|--------|------|
| `Ctrl+Shift+T` | 新建标签页 |
| `Ctrl+Shift+N` | 新建窗口 |
| `Alt+Shift+D` | 新建分屏（垂直） |
| `Alt+Shift+-` | 新建分屏（水平） |
| `Alt+方向键` | 切换分屏焦点 |
| `Ctrl+Shift+W` | 关闭标签页 |
| `Ctrl+Shift+F` | 搜索 |
| `Ctrl+Shift+P` | 命令面板 |
| `Ctrl+,` | 打开设置 |
| `Ctrl+Tab` | 下一个标签页 |
| `Ctrl+Shift+Tab` | 上一个标签页 |
| `Alt+数字键` | 跳转到指定标签页 |

### 自定义快捷键

在 settings.json 的 `"actions"` 数组中添加：

```json
{
    "command": "splitPane",
    "keys": "ctrl+shift+\\"
}
```

## 推荐字体

```powershell
# 安装 Cascadia Code Nerd Font（Microsoft 官方推荐）
winget install Microsoft.CascadiaCode.NF

# 安装 JetBrains Mono Nerd Font
# https://www.nerdfonts.com/font-downloads
```

在 settings.json 中配置字体：

```json
{
    "profiles": {
        "defaults": {
            "font": {
                "face": "CascadiaCode Nerd Font",
                "size": 12
            }
        }
    }
}
```

## 启动设置

```json
{
    "defaultProfile": "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}",
    "launchMode": "maximized",
    "startOnUserLogin": true,
    "alwaysShowTabs": true,
    "showTabsInTitlebar": true
}
```

获取 defaultProfile 的 GUID：`Get-CimInstance -ClassName Win32_Process -Filter "Name='WindowsTerminal.exe'"` 或在设置 UI 中查看。

## 实用配置片段

### 半透明背景

```json
{
    "profiles": {
        "defaults": {
            "useAcrylic": true,
            "acrylicOpacity": 0.8
        }
    }
}
```

### 禁用自动关闭确认

```json
{
    "confirmCloseAllTabs": false
}
```

---

**验证安装**：

```powershell
wt --version
```

**参考**：https://docs.microsoft.com/zh-cn/windows/terminal/
