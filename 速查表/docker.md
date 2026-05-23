# Docker 速查表

> 全平台通用 · 常用命令速查

## 容器生命周期 Container Lifecycle

| 命令 | 说明 |
|------|------|
| `docker run <image>` | 创建并启动容器 |
| `docker run -d --name myapp <image>` | 后台运行并命名 |
| `docker run -it <image> /bin/bash` | 交互式运行（进 shell） |
| `docker run -p 8080:80 <image>` | 端口映射 host:容器 |
| `docker run -v /host:/container <image>` | 挂载卷 |
| `docker run --rm <image>` | 退出后自动删除容器 |
| `docker ps` | 运行中的容器列表 |
| `docker ps -a` | 所有容器列表 |
| `docker stop <container>` | 停止容器 |
| `docker start <container>` | 启动已停止容器 |
| `docker restart <container>` | 重启容器 |
| `docker rm <container>` | 删除容器 |
| `docker rm $(docker ps -aq)` | 删除所有容器 |

## 镜像管理 Image Management

| 命令 | 说明 |
|------|------|
| `docker images` | 列出本地镜像 |
| `docker pull <image>:<tag>` | 拉取镜像 |
| `docker build -t <tag> .` | 从 Dockerfile 构建镜像 |
| `docker build --no-cache -t <tag> .` | 不使用缓存构建 |
| `docker rmi <image>` | 删除镜像 |
| `docker rmi $(docker images -q)` | 删除所有镜像 |
| `docker tag <img> <new>:<tag>` | 给镜像打标签 |

## 信息与统计 Info & Stats

| 命令 | 说明 |
|------|------|
| `docker logs <container>` | 查看容器日志 |
| `docker logs -f <container>` | 实时跟踪日志 |
| `docker logs --tail 100 <container>` | 只看最后 100 行 |
| `docker stats` | 实时资源占用（CPU/内存） |
| `docker inspect <container>` | 查看容器详细信息（JSON） |
| `docker top <container>` | 查看容器内进程 |
| `docker port <container>` | 查看端口映射 |

## 执行与拷贝 Exec & Copy

| 命令 | 说明 |
|------|------|
| `docker exec -it <container> /bin/bash` | 进入运行中的容器 |
| `docker exec <container> <cmd>` | 在容器中执行命令 |
| `docker cp <file> <container>:/path` | 从 host 拷贝到容器 |
| `docker cp <container>:/path <file>` | 从容器拷贝到 host |

## 网络 Network

| 命令 | 说明 |
|------|------|
| `docker network ls` | 列出所有网络 |
| `docker network create <name>` | 创建网络 |
| `docker network connect <net> <container>` | 容器接入网络 |
| `docker network disconnect <net> <container>` | 容器断开网络 |
| `docker run --network=<net> <image>` | 指定网络运行容器 |

## 数据卷 Volume

| 命令 | 说明 |
|------|------|
| `docker volume ls` | 列出卷 |
| `docker volume create <name>` | 创建卷 |
| `docker volume inspect <name>` | 查看卷详情 |
| `docker volume rm <name>` | 删除卷 |
| `docker volume prune` | 删除未使用的卷 |

## Docker Compose

| 命令 | 说明 |
|------|------|
| `docker compose up` | 启动所有服务 |
| `docker compose up -d` | 后台启动 |
| `docker compose down` | 停止并删除容器/网络 |
| `docker compose down -v` | 同上 + 删除卷 |
| `docker compose logs -f` | 实时查看服务日志 |
| `docker compose ps` | 列出 compose 管理的容器 |
| `docker compose exec <svc> <cmd>` | 在服务中执行命令 |
| `docker compose build` | 重新构建镜像 |

## 系统清理 System Prune

| 命令 | 说明 |
|------|------|
| `docker system df` | 查看磁盘使用情况 |
| `docker system prune` | 清理未使用的容器/网络/镜像 |
| `docker system prune -a --volumes` | 深度清理（含未使用镜像/卷） |
| `docker container prune` | 清理已停止容器 |
| `docker image prune -a` | 清理未使用镜像 |
