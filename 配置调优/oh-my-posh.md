# Oh My Posh — PowerShell 提示符定制

> **平台**：Windows（主要） / macOS / Linux（需 pwsh）
> 原名为 Oh My Posh，与 Oh My Zsh 无关

## 安装

### Windows

```powershell
# winget（推荐）
winget install JanDeDobbeleer.OhMyPosh

# 或通过 PowerShell Gallery
Install-Module oh-my-posh -Scope CurrentUser
```

### macOS

```bash
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```

### Linux

```bash
# 一键安装脚本
curl -s https://ohmyposh.dev/install.sh | bash
```

### 验证安装

```powershell
oh-my-posh --version
```

## 配置 Prompt

### 编辑 $PROFILE

```powershell
# 打开 PowerShell 配置文件
notepad $PROFILE

# 或使用 VS Code
code $PROFILE
```

### 添加一行配置

```powershell
# $PROFILE 文件内容：
oh-my-posh init pwsh --config ~/theme.json | Invoke-Expression
```

### 应用配置

```powershell
. $PROFILE
```

## 主题

### 查看内置主题

```powershell
# 列出所有内置主题
Get-PoshThemes

# 实时预览主题
Get-PoshThemes -List
```

### 使用内置主题

```powershell
# 在 $PROFILE 中指定主题名称
oh-my-posh init pwsh --config "$(oh-my-posh --config-path)/themes/1_shell.omp.json" | Invoke-Expression
```

### 推荐主题

| 主题 | 特点 |
|------|------|
| `1_shell` | 简约，单行 |
| `agnoster` | Oh My Zsh agnoster 风格 |
| `atomic` | 原子风格，带 Git 状态 |
| `catppuccin` | 粉紫猫系配色 |
| `dracula` | 经典吸血鬼配色 |
| `montys` | 两行布局，信息完整 |
| `powerlevel10k_rainbow` | 仿 powerlevel10k |
| `star` | 极简带三角形分割 |

完整主题列表：https://github.com/JanDeDobbeleer/oh-my-posh/tree/main/themes

### 自定义主题

```powershell
# 导出当前主题为 JSON
oh-my-posh config export --output ~/mytheme.omp.json

# 编辑后应用到 $PROFILE
oh-my-posh init pwsh --config ~/mytheme.omp.json | Invoke-Expression
```

## Nerd Fonts（必需）

Oh My Posh 使用 Nerd Font 图标，需要安装 Nerd Font：

```powershell
# 查看可安装字体
oh-my-posh font install

# 安装推荐字体
oh-my-posh font install jetbrains-mono

# 在 Windows Terminal 中设置字体
# settings.json: "font.face": "JetBrainsMono Nerd Font"
```

**常见 Nerd Font**：`CaskaydiaCove Nerd Font`、`FiraCode Nerd Font`、`JetBrainsMono Nerd Font`

## Segments（提示符段）

每个主题由 Segment 组成：

| Segment | 显示内容 | 示例 |
|---------|----------|------|
| `time` | 当前时间 | `15:30:45` |
| `os` | 操作系统图标 |  /  / 🐧 |
| `path` | 当前路径 | `~/projects/my-app` |
| `git` | Git 分支+状态 | `main ≡ +1 ~2` |
| `node` | Node.js 版本 | `⬡ v20.10.0` |
| `python` | Python 版本 | `🐍 3.12.0` |
| `go` | Go 版本 | `🐹 1.21.0` |
| `exit` | 上条命令退出码 | `✗ 127` |
| `shell` | Shell 名称 | `pwsh` |
| `executiontime` | 命令执行耗时 | `12ms` |
| `root` | 管理员权限提示 | `#` |
| `npm` | npm 版本 + 包名 | `📦 5.0.0` |
| `yaml` / `json` | 检查当前目录文件 | ... |

### 自定义 Segment 颜色

```powershell
# 在 theme JSON 中设置
{
    "type": "path",
    "style": "powerline",
    "powerline_symbol": "\uE0B0",
    "foreground": "#ffffff",
    "background": "#61AFEF",
    "properties": {
        "style": "folder"
    }
}
```

## 与 Oh My Zsh 相比的优势

| 对比项 | Oh My Posh | Oh My Zsh |
|--------|------------|-----------|
| Shell | PowerShell | Zsh |
| 性能 | 原生二进制，快 | 脚本执行 |
| 配置格式 | JSON | 脚本变量 |
| 实时预览 | `Get-PoshThemes` | 需手动切换 |
| 跨平台 | ✅ | ❌（仅 Unix） |
| 主题丰富度 | 50+ | 200+ |

## 高阶技巧

### 异步渲染（提升性能）

```powershell
oh-my-posh init pwsh --config ~/theme.json --print | Invoke-Expression
# 或在 theme JSON 中设置 "async" 属性
```

### 缓存突破

```powershell
# 刷新 prompt 缓存
Write-PromptCache -Reset
```

### 只在交互式 Shell 启用

```powershell
# $PROFILE
if ($Host.Name -eq 'ConsoleHost') {
    oh-my-posh init pwsh --config ~/theme.json | Invoke-Expression
}
```

---

**参考**：https://ohmyposh.dev/docs
