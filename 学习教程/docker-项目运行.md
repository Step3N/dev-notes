# [链路] Docker 项目运行

> 从零开始，用 Docker 和 docker-compose 跑通完整的 Web 项目

**适用平台**：macOS / Windows / Linux
**前置条件**：
- 有基本命令行经验
- 不需要 Docker 经验

> 💡 **快速上手**：如果只想快速体验，做完 Step 1-3（安装 → 写 Dockerfile → 构建运行）即可。Step 4-6 为进阶编排。

---

## Step 1: 安装 Docker

### macOS

```bash
brew install --cask docker
```

安装后在 Launchpad 打开 Docker，或在终端启动：

```bash
open -a Docker
```

### Windows

```powershell
winget install Docker.DockerDesktop
```

安装后需启用 WSL2 后端：
1. 以管理员身份运行 PowerShell：`wsl --install -d ubuntu`
2. 启动 Docker Desktop → Settings → Resources → WSL Integration → 启用
3. Apply & Restart

### Linux

安装 Docker Engine（非 Docker Desktop）。

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update && sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

Post-install for Linux：

```bash
# 将当前用户加入 docker 组（免 sudo）
sudo usermod -aG docker $USER
newgrp docker   # 或退出重新登录
```

### 验证安装

```bash
docker --version           # 查看版本
docker run hello-world     # 测试能否正常工作（全平台）
```

> 📖 详见 [Docker 安装](../安装软件/docker.md)

---

## Step 2: 编写 Dockerfile

以 Python Flask 应用为例，项目结构：

```
my-flask-app/
├── Dockerfile
├── requirements.txt
├── app.py
└── .dockerignore
```

**Dockerfile**：

```dockerfile
# Dockerfile
FROM python:3.12-slim

WORKDIR /app

# 先复制依赖文件 → 利用缓存，避免每次重装
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 再复制源码
COPY . .

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

**requirements.txt**：

```
flask==3.1.0
gunicorn==23.0.0
```

**app.py**：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello from Docker!'

@app.route('/health')
def health():
    return 'OK'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**.dockerignore**：

```
.git/
__pycache__/
*.pyc
.env
.venv/
```

### 指令说明

| 指令 | 作用 | 要点 |
|------|------|------|
| `FROM` | 指定基础镜像 | 首选 slim/alpine 版本 |
| `WORKDIR` | 设置工作目录 | 后续指令的当前路径 |
| `COPY` | 从构建上下文复制文件 | 先复制不变的依赖 → 利用缓存 |
| `RUN` | 构建时执行命令 | 安装依赖、编译 |
| `EXPOSE` | 声明容器监听端口 | 仅文档用途，实际映射需 `-p` |
| `CMD` | 容器启动命令 | 可被 `docker run` 参数覆盖 |

> 📖 详见 [Dockerfile 语法](../日常使用/dockerfile.md)

---

## Step 3: 构建和运行

```bash
# 构建镜像
docker build -t my-flask-app .

# 运行容器
docker run -d -p 5000:5000 --name myapp my-flask-app

# 验证
curl http://localhost:5000
# 应输出：Hello from Docker!

# 健康检查端点
curl http://localhost:5000/health
# 应输出：OK
```

### 常用操作

```bash
docker ps                  # 查看运行中的容器
docker logs myapp          # 查看日志
docker logs -f myapp       # 实时跟踪日志
docker exec -it myapp sh   # 进入容器内部

# 停止并删除
docker stop myapp && docker rm myapp

# 清理 dangling 镜像
docker image prune -f
```

> 📖 详见 [Docker 常用命令](../日常使用/docker-常用命令.md)

---

## Step 4: docker-compose 多服务编排

在项目根目录创建 `docker-compose.yml`，添加 Redis 服务：

```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

更新 `app.py` 增加 Redis 计数（验证连通性）：

```python
from flask import Flask
import os
import redis

app = Flask(__name__)

r = redis.Redis.from_url(os.getenv('REDIS_URL', 'redis://localhost:6379'))

@app.route('/')
def hello():
    return 'Hello from Docker!'

@app.route('/health')
def health():
    return 'OK'

@app.route('/count')
def count():
    return str(r.incr('counter'))

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

同时安装 Redis 依赖：

```
flask==3.1.0
gunicorn==23.0.0
redis==5.2.1
```

```bash
# 重新构建
docker compose build

# 启动所有服务
docker compose up -d

# 查看服务状态
docker compose ps

# 查看实时日志（指定服务）
docker compose logs -f web

# 验证 Redis 连通性
curl http://localhost:5000/count
# 每次刷新数字 +1

# 停止并清理所有资源
docker compose down

# 停止并同时删除卷
docker compose down -v
```

> 📖 详见 [docker-compose 笔记](../日常使用/docker-compose.md)

---

## Step 5: 配置 Nginx 反向代理

在 `docker-compose.yml` 中添加 Nginx 服务：

```yaml
# docker-compose.yml
version: '3.8'

services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - web

  web:
    build: .
    environment:
      - REDIS_URL=redis://redis:6379
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

在项目根目录创建 `nginx.conf`：

```nginx
# nginx.conf
events {}

http {
    upstream app {
        server web:5000;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://app;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /api/ {
            proxy_pass http://app;
        }
    }
}
```

```bash
# 启动（包含 Nginx）
docker compose up -d

# 通过 Nginx 访问（80 端口），而不是直接 5000 端口
curl http://localhost
# 应输出：Hello from Docker!

# 验证 Nginx 日志
docker compose logs nginx
```

> **macOS/Windows 注意**：80 端口可能被系统进程占用。若报 `port already in use`，将 `80:80` 改为 `8080:80`，访问 `curl http://localhost:8080`。

> 📖 详见 [Nginx 入门配置](../学习教程/nginx.md)

---

## Step 6: 开发 vs 生产环境配置

### 多 compose 文件模式

将公共配置放在 `docker-compose.yml`，环境特定配置放在覆盖文件。

**docker-compose.dev.yml**（开发环境）：

```yaml
version: '3.8'

services:
  web:
    volumes:
      - .:/app       # 挂载源码 → 代码修改即时生效
    environment:
      - FLASK_DEBUG=1
    ports:
      - "5678:5678"  # 调试端口
```

**docker-compose.prod.yml**（生产环境）：

```yaml
version: '3.8'

services:
  web:
    restart: always
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 5s
      retries: 3

  nginx:
    restart: always
```

### 使用方式

```bash
# 开发环境：挂载代码 + 热重载
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d

# 生产环境：不挂载代码 + 健康检查 + 自动重启
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

### .env 文件（环境变量）

```bash
# .env
COMPOSE_PROJECT_NAME=myapp
IMAGE_TAG=latest
```

在 `docker-compose.yml` 中引用：

```yaml
services:
  web:
    image: my-flask-app:${IMAGE_TAG:-latest}
```

```bash
docker compose --env-file .env config   # 验证变量替换
```

### 最佳实践总结

| 方面 | 开发环境 | 生产环境 |
|------|----------|----------|
| 代码挂载 | ✅ volume mount 实现热重载 | ❌ 用构建好的镜像 |
| 调试端口 | ✅ 暴露 debug 端口 | ❌ 不暴露 |
| 日志 | 控制台输出 | 收集到日志系统 |
| 重启策略 | 手动 | `restart: always` |
| 健康检查 | 可选 | 必配 |
| 镜像标签 | `latest` | 语义化版本号 |

---

## 常用命令速查

| 命令 | 说明 |
|------|------|
| `docker ps` | 列出运行中的容器 |
| `docker ps -a` | 列出所有容器（含已停止） |
| `docker images` | 列出本地镜像 |
| `docker system prune -af` | 清理所有未使用的资源 |
| `docker exec -it <container> sh` | 进入运行中的容器 |
| `docker logs -f <container>` | 实时跟踪容器日志 |
| `docker compose ps` | 查看 compose 服务状态 |
| `docker compose logs -f` | 跟踪所有服务日志 |
| `docker compose down -v` | 停止并删除卷 |
| `docker compose build --no-cache` | 强制重新构建（不缓存） |

---

## 常见问题

| 问题 | 原因与解决 |
|------|------------|
| `permission denied` 连 Docker socket | Linux：`sudo usermod -aG docker $USER` 后重新登录 |
| `port already in use` | 旧容器占着端口 → `docker ps` 找到旧容器 → `docker stop <id>` |
| Docker 镜像太大 | 用 slim/alpine 基础镜像；使用多阶段构建；`.dockerignore` 排除无关文件 |
| Docker Desktop 占内存 | macOS/Windows 在 Settings → Resources 限制内存上限 |
| Windows WSL2 磁盘膨胀 | `docker system prune -af` 清理后，用 `wsl --shutdown` 后重新压缩 |
| 容器内 `localhost` 连不上宿主机 | macOS: `host.docker.internal`；Linux: `--network host` 或 `172.17.0.1` |
| `docker compose` 命令找不到 | Docker Compose v2 已集成在 Docker CLI 中，确认安装 `docker-compose-plugin`（Linux） |
| 容器退出（Exit 0） | CMD/ENTRYPOINT 指定的进程执行完就退出。前台进程需持续运行（如 gunicorn 而非 flask run） |

---

## 下一步

- [Node.js 前后端搭建](./nodejs-前后端搭建.md) — 完整的全栈项目搭建
- [Python 深度学习环境搭建](./python-深度学习环境搭建.md) — 在 Docker 中训练深度学习模型
- [Dockerfile 语法](../日常使用/dockerfile.md) — 深入学习 Dockerfile 指令与多阶段构建
- [docker-compose 笔记](../日常使用/docker-compose.md) — 生产级编排配置
