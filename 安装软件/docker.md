# Docker 安装指南

Docker 分为 **Docker Desktop**（含 GUI，全平台）和 **Docker Engine**（仅 CLI，Linux 推荐）。

---

## macOS

推荐使用 Homebrew 安装 Docker Desktop：

```bash
brew install --cask docker
```

安装后在 Launchpad 打开 Docker，或在终端启动：

```bash
open -a Docker
```

验证：

```bash
docker --version
docker run hello-world
```

> 首次运行需等待 Docker Engine 启动（顶部状态栏出现鲸鱼图标）。

---

## Windows

### 方式一：winget（推荐）

```powershell
winget install Docker.DockerDesktop
```

安装后需 **启用 WSL2 后端**：
1. 以管理员身份打开 PowerShell，运行：
   ```powershell
   wsl --install -d ubuntu
   ```
2. 启动 Docker Desktop → Settings → Resources → WSL Integration → 启用
3. Apply & Restart

### 方式二：手动安装

下载 [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)

验证：

```powershell
docker --version
docker run hello-world
```

> **Windows 注意**：必须启用 Hyper-V 和 WSL2。不支持 Windows 10 Home（需用 WSL2）。

---

## Linux

安装 Docker Engine（非 Docker Desktop）。

### Ubuntu / Debian

```bash
# 卸载旧版本
sudo apt remove -y docker docker-engine docker.io containerd runc

# 安装依赖
sudo apt update && sudo apt install -y ca-certificates curl

# 添加官方 GPG 密钥和仓库
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 安装 Docker Engine
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Fedora / RHEL / CentOS

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

验证：

```bash
sudo docker --version
sudo docker run hello-world
```

---

## 非 root 用户运行 Docker（Linux）

安装后，默认需要 `sudo`。将用户加入 `docker` 组可免除：

```bash
sudo usermod -aG docker $USER
newgrp docker   # 或重新登录
```

验证：

```bash
docker run hello-world   # 无需 sudo
```

> **安全提示**：`docker` 组等价于 root 权限，仅添加信任用户。

---

## WSL2 集成详解（Windows）

Docker Desktop 在 Windows 上推荐 WSL2 后端（性能优于 Hyper-V）。

```powershell
# 确保 WSL2 为默认版本
wsl --set-default-version 2

# 在 Docker Desktop 中启用 WSL 集成后，在 WSL 终端内直接使用 docker
wsl --set-version Ubuntu 2
```

在 WSL2 内部无需单独安装 Docker Engine——Docker Desktop 会自动代理。

验证：

```bash
# 在 WSL 中运行
docker ps
```

---

## 卸载

| 平台 | 命令 |
|------|------|
| **macOS** | `brew uninstall --cask docker` |
| **Windows** | `winget uninstall Docker.DockerDesktop` |
| **Linux** | `sudo apt remove -y docker-ce docker-ce-cli` |

---

## 总结验证

所有平台执行以下命令确认安装成功：

```bash
docker --version
docker info
docker run --rm hello-world
```

预期输出包含 Docker 版本号和 "Hello from Docker!" 消息。
