# Windows 开发者必备软件清单

## 开发工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **Windows Terminal** | 微软官方终端，支持多 Tab、GPU 加速、自定义主题 | `winget install Microsoft.WindowsTerminal` |
| **VS Code** | 代码编辑器标配，Remote 插件连 WSL/SSH 无敌 | `winget install Microsoft.VisualStudioCode` |
| **JetBrains Toolbox** | 管理 IntelliJ/GoLand/PyCharm 等 IDE 版本 | `winget install JetBrains.Toolbox` |
| **Docker Desktop** | 容器化开发，搭配 WSL2 后端性能极佳 | `winget install Docker.DockerDesktop` |
| **WSL2 + Ubuntu** | Windows 原生运行 Linux，开发环境核心基础设施 | `wsl --install`（管理员 PowerShell） |
| **Postman** | API 调试工具，支持集合/环境/自动化测试 | `winget install Postman.Postman` |
| **TablePlus** | 原生 SQL 客户端，支持主流数据库 | `winget install TablePlus.TablePlus` |
| **Git for Windows** | 含 Git Bash、Git GUI，版本控制必备 | `winget install Git.Git` |
| **DevToys** | 开发者瑞士军刀：JSON 格式化、Base64 编码、正则测试等 | `winget install DevToys.DevToys` |

## 系统工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **PowerToys** | 微软官方工具箱：FancyZones（窗口布局）、PowerRename、颜色选择器 | `winget install Microsoft.PowerToys` |
| **Everything** | 文件名秒级全盘搜索，比 Windows 自带快百倍 | `winget install voidtools.Everything` |
| **Notepad++** | 轻量文本编辑器，大文件打开快，语法高亮 | `winget install Notepad++.Notepad++` |
| **7-Zip** | 免费压缩解压，支持 7z/ZIP/RAR 等 | `winget install 7zip.7zip` |
| **TreeSize Free** | 磁盘空间分析，可视化展示大文件分布 | `winget install JAMSoftware.TreeSize.Free` |
| **Snipaste** | 截图 + 贴图工具，截图后可以钉在桌面上 | `winget install Snipaste.Snipaste` |
| **ShareX** | 开源截屏/录屏，内置 OCR、图像编辑 | `winget install ShareX.ShareX` |

## 效率工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **Obsidian** | 本地 Markdown 笔记，知识图谱 | `winget install Obsidian.Obsidian` |
| **Chrome** | 开发者工具标准浏览器 | `winget install Google.Chrome` |
| **Firefox** | 隐私友好，开发者工具够用 | `winget install Mozilla.Firefox` |
| **1Password** | 密码管理器，Windows Hello 解锁 | `winget install AgileBits.1Password` |
| **AutoHotkey** | 自动化脚本，热键映射、文本模板、窗口控制 | `winget install AutoHotkey.AutoHotkey` |
| **Ditto** | 剪贴板历史管理器，支持搜索和快捷键粘贴 | `winget install Ditto.Ditto` |

## 终端美化

| 软件 | 说明 | 安装 |
|------|------|------|
| **Oh My Posh** | PowerShell 提示符美化，主题丰富 | `winget install JanDeDobbeleer.OhMyPosh` |
| **Nerd Fonts** | 开发者字体合集（FiraCode/JetBrains Mono），含图标 | `winget install DevComrade.NerdFonts` |
| **clink** | CMD 获得 Bash 式自动补全、行编辑 | `winget install clink.clink` |
| **Scoop** | Windows 命令行包管理器（类 brew） | PowerShell 安装（见官网） |

## 通讯工具

| 软件 | 说明 | 安装 |
|------|------|------|
| **Slack** | 团队协作 | `winget install Slack.Slack` |
| **Telegram** | 安全即时通讯 | `winget install Telegram.TelegramDesktop` |
| **WeChat** | 微信桌面版 | 官网下载安装包 |
| **Discord** | 游戏/开发者社群 | `winget install Discord.Discord` |

> 多数软件可用 `winget install` 一键安装。先确保已安装 `winget`（Windows 10 1809+ / Windows 11 自带）。
> WSL2 启用后，Linux 工具链（apt/git/node/docker）可在 Ubuntu 子系统中使用。
