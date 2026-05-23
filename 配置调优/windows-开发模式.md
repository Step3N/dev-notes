# Windows — 开发者模式配置

Windows 默认设置偏向普通用户，以下配置把系统调成适合开发的姿势。

---

## 🔧 开启开发者模式

```powershell
# 方式 1：设置界面
# 设置 → 隐私和安全性 → 开发者选项 → 开发者模式 → 开启

# 方式 2：注册表
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v AllowDevelopmentWithoutDevLicense /d 1

# 方式 3：PowerShell（Windows 10 1803+）
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v AllowDevelopmentWithoutDevLicense /d 1
```

> 开发者模式允许 sideload 应用、使用 `ssh` 客户端、设备门户等功能。

---

## 🐧 WSL2

详见 `wsl2-完整配置笔记.md`，此处只记关键命令：

```powershell
# 启用 WSL 功能
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 启用虚拟机平台（WSL2 所需）
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 重启后，设置默认版本
wsl --set-default-version 2
```

---

## 🪟 Windows Terminal

```powershell
# 通过 winget 安装
winget install Microsoft.WindowsTerminal

# 或 scoop
scoop bucket add extras
scoop install windows-terminal
```

> 配置推荐：字体选 `Cascadia Code`、默认 profile 设为 Ubuntu、开启 acrylic 毛玻璃。

---

## 📏 启用长路径支持

Windows 默认路径限制 260 字符，很多前端项目会踩坑。

```powershell
# 方式 1：组策略（推荐）
# 运行 gpedit.msc → 计算机配置 → 管理模板 → 系统 → 文件系统 → 启用 Win32 长路径

# 方式 2：注册表
reg add "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" /v LongPathsEnabled /t REG_DWORD /d 1 /f

# 验证
reg query "HKLM\SYSTEM\CurrentControlSet\Control\FileSystem" /v LongPathsEnabled
# 应看到 LongPathsEnabled  REG_DWORD  0x1
```

---

## ⏱️ 禁用快速启动

双系统（Windows + Linux）用户必做，否则 Linux 无法正常挂载 NTFS 分区。

```powershell
# 方式 1：设置
# 控制面板 → 电源选项 → 选择电源按钮的功能 → 更改当前不可用的设置
# → 取消勾选"启用快速启动"

# 方式 2：PowerShell
powercfg /h off
```

> 快速启动本质是混合休眠（hiberboot），在 SSD 上开机差异很小，关了更安全。

---

## 👁️ 显示隐藏文件 & 扩展名

```powershell
# 显示隐藏文件
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v Hidden /t REG_DWORD /d 1 /f

# 显示已知文件扩展名
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v HideFileExt /t REG_DWORD /d 0 /f

# 重启资源管理器
taskkill /f /im explorer.exe && start explorer.exe
```

---

## 📜 PowerShell 执行策略

```powershell
# 查看当前策略
Get-ExecutionPolicy
# Restricted — 默认，禁止任何脚本

# 设为 RemoteSigned（推荐）：本地脚本可运行，远程脚本需签名
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser

# 验证
Get-ExecutionPolicy
# 应输出: RemoteSigned
```

> `RemoteSigned` 保证你能运行自己写的 `.ps1` 脚本，同时防止未签名远程脚本自动运行。

---

## 🧰 其他推荐设置

```powershell
# 关闭 Xbox Game Bar（减少后台进程）
Get-AppxPackage *xboxapp* | Remove-AppxPackage

# 关闭"粘贴时弹出建议"（Windows 11 剪贴板建议）
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowCopilotButton /t REG_DWORD /d 0 /f

# 任务栏居左（如果你习惯 macOS/Linux 风格）
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarAl /t REG_DWORD /d 0 /f
# 0=左对齐, 1=居中

# 关闭粘滞键提示（按 5 次 Shift 不再弹窗）
reg add "HKCU\Control Panel\Accessibility\StickyKeys" /v Flags /t REG_SZ /d "506" /f
```

---

## ✅ 验证清单

| 项目 | 验证方式 |
|------|---------|
| 开发者模式 | 设置中显示已开启 |
| WSL2 | `wsl --status` 显示版本 2 |
| Windows Terminal | 开始菜单可搜到 |
| 长路径 | 新建 `C:\a\...（超过 260 字符）\` 文件夹不报错 |
| 快速启动 | `powercfg /a` 不显示"快速启动" |
| 隐藏文件 | C 盘根目录应能看到 `ProgramData` |
| 执行策略 | `Get-ExecutionPolicy` 输出 `RemoteSigned` |

---

## 🔗 参考

- https://learn.microsoft.com/zh-cn/windows/apps/get-started/enable-your-device-for-development
- https://learn.microsoft.com/en-us/windows/wsl/
