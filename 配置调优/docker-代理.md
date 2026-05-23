# Docker 代理配置

Docker 的代理分两个层面：

- **Daemon 代理** — 影响 `docker pull` / `docker build` 拉取镜像
- **Container 代理** — 影响容器内部程序访问外网

---

## Daemon 代理（镜像拉取）

### Linux（systemd 管理）

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
```

创建 `/etc/systemd/system/docker.service.d/proxy.conf`：

```ini
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,.local,.localdomain"
```

重载并重启：

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

验证：

```bash
sudo systemctl show --property=Environment docker
docker info | grep -i proxy
```

### macOS / Windows（Docker Desktop）

GUI 操作路径：

```
Docker Desktop → Settings → Resources → Proxies
```

选择 **Manual proxy configuration**，填入：

| 字段 | 值 |
|------|-----|
| HTTP Proxy | `http://127.0.0.1:7890` |
| HTTPS Proxy | `http://127.0.0.1:7890` |
| No Proxy | `localhost,127.0.0.1,.local` |

点击 **Apply & Restart**。

---

## Container 代理（容器内部）

### docker-compose 方式

```yaml
version: '3'
services:
  app:
    image: node:18
    environment:
      - HTTP_PROXY=http://host.docker.internal:7890
      - HTTPS_PROXY=http://host.docker.internal:7890
      - NO_PROXY=localhost,127.0.0.1
```

> `host.docker.internal` 是 Docker 提供的魔法主机名，指向宿主机。

### docker run 方式

```bash
docker run -e HTTP_PROXY=http://host.docker.internal:7890 \
           -e HTTPS_PROXY=http://host.docker.internal:7890 \
           -e NO_PROXY=localhost,127.0.0.1 \
           alpine curl -s https://www.google.com
```

### Dockerfile 方式（构建时写入）

```dockerfile
FROM alpine:3.19
ENV HTTP_PROXY=http://host.docker.internal:7890
ENV HTTPS_PROXY=http://host.docker.internal:7890
ENV NO_PROXY=localhost,127.0.0.1
```

---

## Build 时临时代理（不写入镜像）

```bash
docker build \
  --build-arg HTTP_PROXY=http://proxy:7890 \
  --build-arg HTTPS_PROXY=http://proxy:7890 \
  --build-arg NO_PROXY=localhost,127.0.0.1 \
  -t my-image .
```

> `--build-arg` 只在构建过程中生效，不会留在最终镜像的层中（除非 Dockerfile 显式 `ARG` + `ENV`）。

---

## 验证代理状态

```bash
# 查看 daemon 代理设置
docker info | grep -i proxy

# 容器内部测试
docker run --rm alpine wget -q -O- https://httpbin.org/ip

# 查看容器实际出口 IP
docker run --rm alpine sh -c "wget -q -O- https://httpbin.org/ip"
```

---

## 常见问题

### `Error response from daemon: ... no such host`

- 检查 daemon 代理是否配置正确
- Linux 上检查 `systemctl status docker` 是否有报错

### 容器内无法解析 `host.docker.internal`

- **Linux**：`host.docker.internal` 默认不支持，需使用 `--add-host`：

```bash
docker run --add-host host.docker.internal:host-gateway ...
```

或在 docker-compose 中添加：

```yaml
extra_hosts:
  - "host.docker.internal:host-gateway"
```

- **macOS / Windows**：默认支持 `host.docker.internal`

### docker pull 太慢但已配代理

检查代理客户端是否支持 HTTPS CONNECT。某些代理（如 CNI 插件）需要额外配置。

---

## 平台差异

| 配置项 | **Linux** | **macOS** | **Windows** |
|--------|-----------|-----------|-------------|
| Daemon 代理 | systemd drop-in | Docker Desktop GUI | Docker Desktop GUI |
| Docker Engine | `/etc/docker/daemon.json` | Docker Desktop | Docker Desktop |
| `host.docker.internal` | `--add-host` 或 自 Docker 20.10+ 可用 | 原生支持 | 原生支持 |
| 配置文件路径 | `/etc/systemd/system/docker.service.d/proxy.conf` | GUI 管理 | GUI 管理 |
