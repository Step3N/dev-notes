# WSL2 — 完整配置笔记

WSL2 让 Windows 有了真正的 Linux 内核，开发体验接近原生。

---

## 📦 启用 WSL 功能

```powershell
# 以管理员身份运行 PowerShell

# 启用 WSL 功能
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 启用虚拟机平台（WSL2 核心依赖）
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# 重启
Restart-Computer
```

---

## 🖥️ 安装 WSL2 内核更新

```powershell
# 下载并安装 WSL2 Linux 内核更新包
# 下载地址:
# https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

# 或通过 winget 安装 WSL（已包含内核）
winget install Microsoft.WSL
```

> 安装内核更新包是让 WSL2 正常工作的关键步骤，很多人卡在这一步。

---

## ⚙️ 设置 WSL2 为默认

```powershell
# 设置默认版本
wsl --set-default-version 2

# 验证
wsl --status
# 应看到: Default Version: 2

# 如果已有 WSL1 的发行版，升级到 WSL2
wsl --set-version <发行版名称> 2
# 查看发行版名称: wsl -l -v
```

---

## 🐧 安装 Linux 发行版

```powershell
# 列出可用发行版
wsl --list --online

# 安装 Ubuntu（推荐）
wsl --install -d Ubuntu

# 安装后会自动启动，设置用户名和密码
# 用户名: 你自己选，不需要和 Windows 用户名一致
# 密码: Linux 的 sudo 密码，可以设短一些
```

安装完成后首次启动：

```bash
# 更新包
sudo apt update && sudo apt upgrade -y

# 验证内核版本
uname -r
# 应包含: microsoft-standard-WSL2
```

---

## 📋 WSL 配置文件 — `.wslconfig`

在 Windows 用户目录 `%USERPROFILE%` 下创建 `.wslconfig`，控制所有 WSL2 发行版的全局资源：

```ini
# %USERPROFILE%\.wslconfig
[wsl2]
memory=8GB            # 最大内存（默认宿主机 50%，建议 4-8GB）
processors=4          # 最大 CPU 核心数
localhostForwarding=true
swap=2GB              # 交换分区大小
swapFile=//wsl/swap   # swap 文件位置
kernelCommandLine=vsyscall=emulate
```

> 修改后需重启 WSL：`wsl --shutdown` 再重新打开终端。

---

## 🔄 Interop（进程互操作）

默认开启，控制 WSL 与 Windows 之间的进程互调用：

| 功能 | 说明 | 文件路径 |
|------|------|---------|
| Interop Enabled | 可在 WSL 中启动 Windows 进程 | `/etc/wsl.conf` |
| Append Windows Path | Windows PATH 自动注入 WSL | `/etc/wsl.conf` |

```ini
# /etc/wsl.conf (在 WSL 内部)
[interop]
enabled = true
appendWindowsPath = true

[user]
default = your-username

[boot]
systemd = true
```

---

## 📂 跨文件系统访问

### 从 WSL 访问 Windows 文件

```bash
# Windows C 盘挂载在 /mnt/c
ls /mnt/c/Users/your-windows-username/Desktop

# 在 WSL 中编辑 Windows 文件
code /mnt/c/项目路径
```

> ⚠️ 性能警告：在 `/mnt/c` 下读写速度慢。**项目代码务必放在 WSL 内部**（`/home/xxx/`），用 `\\wsl$` 从 Windows 访问。

### 从 Windows 访问 WSL 文件

在文件资源管理器地址栏输入：

```
\\wsl$\Ubuntu\home\用户名
```

> 可以映射网络驱动器，或者直接用 VS Code 的 Remote-WSL 扩展。

---

## 🌟 Systemd 支持

WSL2 自 0.67.6+ 支持 systemd：

```bash
# 1. 确保 WSL 版本 >= 0.67.6
wsl --version

# 2. 编辑 /etc/wsl.conf
sudo tee -a /etc/wsl.conf << 'EOF'
[boot]
systemd = true
EOF

# 3. 关闭并重新启动 WSL
wsl.exe --shutdown
# 重新打开 WSL 终端
```

验证：

```bash
systemctl list-units --type=service --state=running
# 应能看到 systemd 管理的服务列表
```

---

## 🌐 代理配置

WSL 共享 Windows 的代理：

```bash
# 在 ~/.zshrc 或 ~/.bashrc 中添加
# 自动获取 Windows 宿主机 IP（WSL2 的 /etc/resolv.conf 中的 nameserver）
export host_ip=$(awk '/nameserver/ {print $2}' /etc/resolv.conf)
export ALL_PROXY="http://$host_ip:7890"
export HTTP_PROXY="$ALL_PROXY"
export HTTPS_PROXY="$ALL_PROXY"
export NO_PROXY="localhost,127.0.0.1,.local"

# 代理端口替换为你的代理软件端口（Clash: 7890, v2ray: 10809）
```

> 如果使用 clash-verge / v2rayN，需开启"允许局域网连接"。

---

## 🐳 Docker 集成

```bash
# 方案 1：Docker Desktop（推荐新手）
# Windows 上安装 Docker Desktop，设置中启用 WSL2 Backend
# Settings → Resources → WSL Integration → 启用你的发行版

# 方案 2：WSL 内部安装 Docker Engine（省资源）
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER

# 启动 Docker
sudo systemctl enable docker && sudo systemctl start docker

# 验证
docker run hello-world
```

---

## ⚠️ 常见问题

### Vmmem 进程占用内存过高

```powershell
# 方案 1：限制 .wslconfig 中的 memory
# 方案 2：手动回收
wsl --shutdown
# .wslconfig 中设置 memory=4GB 限死上限
```

### WSL 网络不通 / DNS 解析失败

```bash
# 重置 WSL 网络
wsl.exe --shutdown

# Windows 中以管理员运行：
netsh winsock reset
netsh int ip reset all
netsh winhttp reset proxy
ipconfig /flushdns
# 重启 Windows
```

### WSL 时间与 Windows 不同步

```bash
# WSL 内手动同步
sudo hwclock -s
# 或安装 ntp
sudo apt install ntpdate
sudo ntpdate time.windows.com
```

### WSL 无法启动 / 报 0x80370102

```powershell
# 确保 BIOS 开启虚拟化（VT-x / AMD-V）
# 在 Windows 中检查：
systeminfo | findstr "虚拟化"
# 应显示: 已启用

# 如果未启用，重启进 BIOS 开启虚拟化技术
```

---

## ✅ 验证清单

| 项目 | 验证方式 |
|------|---------|
| WSL2 版本 | `wsl --status` |
| 内核版本 | `uname -r` 含 `microsoft` |
| 文件互通 | `ls /mnt/c` 能看到 Windows C 盘 |
| Systemd | `systemctl --version` 正常 |
| Docker | `docker ps` 不报错 |
| 代理 | `curl -I https://google.com` 返回 200 |

---

## 🔗 参考

- https://learn.microsoft.com/en-us/windows/wsl/
- https://github.com/microsoft/WSL
- https://docs.docker.com/desktop/wsl/
