# winget — Windows 包管理器

Windows 官方命令行包管理器，全称 **Windows Package Manager CLI**。装软件不再需要满网找安装包。

---

## 📦 安装

**方式一：从 Microsoft Store 安装**

搜索 "App Installer" → 安装 → 自带 winget。

**方式二：官方 GitHub**

```powershell
# 下载最新版 App Installer 并安装
# https://github.com/microsoft/winget-cli/releases
```

**方式三：Windows 11 / 最新 Windows 10 已内置**

```powershell
winget --version
```

验证安装：

```powershell
winget --version
# 输出示例: v1.9.x
```

---

## ⚙️ 常用命令

| 操作 | 命令 |
|------|------|
| 搜索软件 | `winget search <关键词>` |
| 安装 | `winget install <包名>` |
| 卸载 | `winget uninstall <包名>` |
| 升级单个 | `winget upgrade <包名>` |
| 升级全部 | `winget upgrade --all` |
| 列出已装 | `winget list` |
| 查看详情 | `winget show <包名>` |
| 导出清单 | `winget export -o packages.json` |
| 导入清单 | `winget import -i packages.json` |

---

## 🔧 安装示例

```powershell
# 安装开发工具
winget install Microsoft.VisualStudioCode
winget install Git.Git
winget install OpenJS.NodeJS

# 安装日常应用
winget install Google.Chrome
winget install 7zip.7zip
winget install VideoLAN.VLC

# 指定版本
winget install Microsoft.PowerShell --version 7.4.0
```

> 包名不唯一时用 `--id` 精准指定：
> ```powershell
> winget install --id Microsoft.VisualStudioCode
> ```

---

## 📝 管理源（Sources）

列出当前源：

```powershell
winget source list
```

默认源：
- **winget** — 微软官方仓库
- **msstore** — Microsoft Store

添加第三方源：

```powershell
winget source add myrepo https://example.com/repo
```

---

## 💡 Export / Import

备份已安装软件：

```powershell
winget export -o C:\Users\<用户名>\winget-packages.json
```

在另一台电脑恢复：

```powershell
winget import -i C:\Users\<用户名>\winget-packages.json
```

---

## ⚠️ 常见问题

### 找不到包

```powershell
# 先搜索确认包名
winget search python

# 用 --id 精确匹配
winget install --id Python.Python.3.12
```

### 安装了但没加到 PATH

部分软件需要重启终端，或手动添加 PATH。

### 安装失败 — 被 antivirus 拦截

```powershell
# 临时关闭实时保护后再试
```

### 更新源缓存

```powershell
winget source update
```

### 安装 msstore 源上的包

```powershell
# msstore 源默认禁用，先启用
winget source enable msstore
winget install "9NBLGGH4NNS1"  # 示例: Windows Terminal
```

---

## 🔗 参考

- 官方文档: https://learn.microsoft.com/windows/package-manager/winget
- GitHub: https://github.com/microsoft/winget-cli
