# Podman vs Docker

Podman 是由 Red Hat 开发的容器引擎，兼容 Docker CLI，但**无守护进程、默认 rootless**。

---

## 核心对比

| 特性 | Docker | Podman |
|------|--------|--------|
| 架构 | C/S：`docker CLI` → `dockerd` 守护进程 | 无守护进程，直接 fork-containers |
| 根权限 | 默认需要 root（或 docker 组） | 默认 rootless（无需 sudo） |
| 启动方式 | `systemctl start docker` | 无需启动服务 |
| 容器启动者 | dockerd | 当前用户 |
| Pod 支持 | 无原生 pod 概念 | 支持 pod（类似 Kubernetes） |
| Kubernetes YAML | 需第三方工具 | 原生支持 `podman generate kube` |
| Docker Hub 镜像 | 默认 | 默认（兼容） |
| 构建镜像 | `docker build` | `podman build`（兼容） |
| Docker Compose | 原生支持 | 需 `podman-compose` 或兼容模式 |
| 桌面端 | Docker Desktop | Podman Desktop |
| Rootless 网络 | 较复杂 | 内置 slirp4netns / pasta |

---

## 安装 Podman

### macOS

```bash
brew install podman

# 初始化并启动 Podman 虚拟机（macOS 需要 Linux VM）
podman machine init
podman machine start
```

### Windows

```powershell
winget install RedHat.Podman

# 初始化虚拟机
podman machine init
podman machine start
```

### Linux

```bash
# Ubuntu / Debian
sudo apt install -y podman

# Fedora（内置）
sudo dnf install -y podman

# 或使用官方仓库
. /etc/os-release
printf "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/xUbuntu_${VERSION_ID}/ /\n" | sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
curl -fsSL "https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable/xUbuntu_${VERSION_ID}/Release.key" | sudo gpg --dearmor -o /etc/apt/keyrings/podman.gpg
sudo apt update && sudo apt install -y podman
```

验证：

```bash
podman --version
podman run hello-world
```

---

## 一键迁移：alias docker=podman

Podman CLI 完全兼容 Docker CLI，直接设置别名即可：

```bash
alias docker=podman
```

持久化（写入 `~/.zshrc` 或 `~/.bashrc`）：

```bash
echo "alias docker=podman" >> ~/.zshrc
source ~/.zshrc
```

迁移后测试：

```bash
docker --version
# 输出: podman version 5.x.x

docker run -d -p 8080:80 nginx:alpine
docker ps
docker exec -it <container> sh
```

> **注意**：别名仅影响交互式终端。脚本或 CI/CD 中需显式设置。

---

## Podman Desktop

Podman Desktop 是 Docker Desktop 的开源替代，提供 GUI。

```bash
# macOS
brew install --cask podman-desktop

# Windows
winget install RedHat.PodmanDesktop
```

功能：
- 查看容器/镜像/卷
- 启动/停止容器
- Kubernetes 集成
- 可切换连接到 Docker Engine（兼容模式）

---

## Kubernetes YAML 生成

Podman 原生支持从容器生成 Kubernetes YAML：

```bash
# 运行容器
podman run -d --name my-app -p 5000:5000 my-image

# 生成 K8s YAML
podman generate kube my-app > deployment.yaml

# 查看内容
cat deployment.yaml
```

Docker 需额外工具：

```bash
# Docker 方式
docker run -d --name my-app -p 5000:5000 my-image
docker inspect my-app > inspect.json
# 然后使用 kompose 转换
kompose convert -f docker-compose.yml
```

---

## Pod 概念（Pod vs 单容器）

Pod 是 Kubernetes 的基本调度单元，Podman 原生支持：

```bash
# 创建 pod
podman pod create --name web-pod -p 3000:3000

# 在 pod 中启动容器（共享网络/命名空间）
podman run -d --pod web-pod --name app my-app
podman run -d --pod web-pod --name sidecar my-sidecar

# 列出 pod
podman pod ps
```

---

## 何时使用 Podman

| 推荐 Podman | 推荐 Docker |
|-------------|-------------|
| 安全敏感环境（rootless 默认） | 团队已深度依赖 Docker Compose |
| 无需守护进程的场景 | 需要 Docker Desktop GUI |
| 开发 K8s 应用（原生 pod/yaml） | Windows/macOS 用户习惯 Docker Desktop |
| CI/CD 无需 root 权限 | 已有大量 Docker 化项目 |
| 单机容器管理 | 需要 Swarm 编排 |

---

## Docker 独有的功能

Podman 不支持：
- **Docker Swarm**（编排集群）
- **Docker Desktop 内置 Kubernetes**
- **BuildKit 高级缓存**（Podman Buildah 部分兼容）

---

## 快速诊断

```bash
# 确认当前是否在使用 Podman
docker info | head -5
# 输出含 "podman" 字样即为 Podman

# 检查 rootless 状态
podman info | grep rootless
```
