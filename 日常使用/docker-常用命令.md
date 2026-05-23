# Docker 常用命令

> 全平台通用。`<>` 表示占位符，使用时替换。

---

## 镜像管理

| 命令 | 说明 |
|------|------|
| `docker pull <image>:<tag>` | 拉取镜像 |
| `docker images` | 列出本地镜像 |
| `docker build -t <name>:<tag> .` | 构建镜像 |
| `docker rmi <image>` | 删除镜像 |
| `docker image prune` | 清理未使用的镜像 |

```bash
# 拉取官方镜像
docker pull nginx:alpine
docker pull postgres:16
docker pull python:3.12-slim

# 查看镜像
docker images

# 删除镜像
docker rmi nginx:alpine

# 清理所有 dangling 镜像
docker image prune -f

# 清理所有未使用的镜像（谨慎）
docker image prune -a
```

---

## 容器生命周期

| 命令 | 说明 |
|------|------|
| `docker run <image>` | 创建并启动容器 |
| `docker ps` | 列出运行中的容器 |
| `docker ps -a` | 列出所有容器（含已停止） |
| `docker stop <container>` | 停止容器 |
| `docker start <container>` | 启动已停止的容器 |
| `docker restart <container>` | 重启容器 |
| `docker rm <container>` | 删除容器 |
| `docker exec -it <container> <cmd>` | 在运行中的容器内执行命令 |
| `docker logs <container>` | 查看容器日志 |

### 常用 flags

| Flag | 说明 |
|------|------|
| `-d` | 后台运行（detach） |
| `-it` | 交互式终端 |
| `--rm` | 容器停止后自动删除 |
| `-p <host>:<container>` | 端口映射 |
| `-e KEY=VALUE` | 设置环境变量 |
| `--name <name>` | 指定容器名称 |
| `-v <host>:<container>` | 挂载卷 |
| `--restart always` | 自动重启策略 |

```bash
# Nginx - 前台运行
docker run --rm -p 8080:80 nginx:alpine

# Nginx - 后台运行
docker run -d --name my-nginx -p 8080:80 nginx:alpine

# PostgreSQL - 带环境变量和持久化
docker run -d \
  --name my-postgres \
  -e POSTGRES_PASSWORD=mysecret \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16

# Python - 交互式运行脚本
docker run --rm -v "$(pwd):/app" -w /app python:3.12-slim python my_script.py

# 进入运行中的容器
docker exec -it my-nginx sh

# 查看日志
docker logs -f my-nginx
```

---

## 卷管理

| 命令 | 说明 |
|------|------|
| `docker volume create <name>` | 创建卷 |
| `docker volume ls` | 列出卷 |
| `docker volume rm <name>` | 删除卷 |
| `docker volume prune` | 删除未使用的卷 |

### `-v` vs `--mount`

```bash
# -v 语法（简洁）
docker run -v mydata:/data alpine

# --mount 语法（更详细，推荐用于复杂场景）
docker run --mount source=mydata,target=/data alpine

# 绑定挂载（宿主机目录）
docker run -v /host/path:/container/path alpine
# 等效于
docker run --mount type=bind,source=/host/path,target=/container/path alpine
```

---

## 网络管理

| 命令 | 说明 |
|------|------|
| `docker network ls` | 列出网络 |
| `docker network create <name>` | 创建网络 |
| `docker network connect <net> <container>` | 将容器连接到网络 |
| `docker network disconnect <net> <container>` | 断开容器与网络的连接 |

```bash
# 创建 bridge 网络
docker network create my-network

# 在自定义网络中启动容器（容器名可作为 DNS）
docker run -d --name app --network my-network my-app
docker run -d --name db --network my-network postgres:16

# 应用可通过 "db" 主机名连接数据库
```

---

## 系统管理

| 命令 | 说明 |
|------|------|
| `docker system df` | 查看磁盘使用情况 |
| `docker system prune` | 清理未使用的资源（容器、网络、镜像、构建缓存） |
| `docker system prune -a` | 更彻底地清理（含未使用的镜像） |

```bash
# 磁盘使用统计
docker system df

# 一键清理
docker system prune -af
```

---

## 完整示例：Web 应用开发

```bash
# 1. 拉取镜像
docker pull python:3.12-slim

# 2. 创建网络
docker network create dev-net

# 3. 启动 PostgreSQL
docker run -d \
  --name dev-db \
  --network dev-net \
  -e POSTGRES_PASSWORD=devpass \
  -e POSTGRES_DB=appdb \
  -v dev-pgdata:/var/lib/postgresql/data \
  postgres:16

# 4. 构建应用镜像
docker build -t my-app .

# 5. 启动应用
docker run -d \
  --name my-app \
  --network dev-net \
  -p 5000:5000 \
  -e DATABASE_URL=postgresql://postgres:devpass@dev-db/appdb \
  my-app

# 6. 查看日志
docker logs -f my-app

# 7. 进入数据库容器
docker exec -it dev-db psql -U postgres -d appdb
```
