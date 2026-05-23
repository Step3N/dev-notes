# Docker Compose 笔记

> 全平台通用。Docker Compose 用于定义和运行多容器应用。

---

## 版本说明

从 Docker Desktop 和 Docker Engine 25+ 开始，`docker compose`（v2，无连字符）为内置命令。

| 形式 | 说明 |
|------|------|
| `docker compose` | **v2 推荐**，Go 实现，作为 Docker CLI 插件 |
| `docker-compose` | 旧版 v1，Python 实现，需单独安装 |

```bash
# 检查 v2 是否可用
docker compose version

# 如不可用，安装
sudo apt install -y docker-compose-plugin
```

---

## YAML 结构

```yaml
services:
  service_name:
    image: ...
    build: ...
    ports: ...
    environment: ...
    volumes: ...
    depends_on: ...
    restart: ...

volumes:
  volume_name:

networks:
  network_name:
```

---

## 完整示例：Flask + PostgreSQL

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: postgresql://postgres:secret@db:5432/appdb
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - .:/app
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: appdb
    volumes:
      - pgdata:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5
    restart: unless-stopped

volumes:
  pgdata:
```

### 启动

```bash
# 构建并启动
docker compose up -d

# 仅构建
docker compose build

# 查看状态
docker compose ps

# 查看日志
docker compose logs -f
docker compose logs -f web

# 执行命令（在运行中的容器内）
docker compose exec web sh
docker compose exec db psql -U postgres -d appdb

# 停止并移除
docker compose down

# 停止并移除卷（⚠️ 数据丢失）
docker compose down -v

# 重启服务
docker compose restart
```

---

## 常用 keywords

### image / build

```yaml
services:
  app:
    # 使用已有镜像
    image: nginx:alpine

    # 或从 Dockerfile 构建
    build:
      context: .
      dockerfile: Dockerfile
      args:
        VERSION: "1.0"
```

### ports

```yaml
ports:
  - "8080:80"        # host:container
  - "80:80"          # 简写
  - "3000-3005:3000-3005"  # 端口范围
```

### environment

```yaml
# 方式一：直接列表
environment:
  - DEBUG=true
  - DB_HOST=db

# 方式二：映射形式
environment:
  DEBUG: "true"
  DB_HOST: "db"

# 方式三：从 .env 文件（见下文）
```

### volumes

```yaml
volumes:
  # 命名卷（推荐持久化）
  - pgdata:/var/lib/postgresql/data

  # 绑定挂载（开发热重载）
  - ./src:/app/src

  # 只读
  - ./config:/app/config:ro
```

### depends_on

```yaml
depends_on:
  db:
    condition: service_healthy
  redis:
    condition: service_started

# 或简单形式（仅控制启动顺序，不等待就绪）
depends_on:
  - db
  - redis
```

### restart

| 策略 | 说明 |
|------|------|
| `no` | 不自动重启（默认） |
| `always` | 总是重启 |
| `on-failure` | 仅退出码非零时重启 |
| `unless-stopped` | 除非手动停止，否则重启 |

---

## .env 文件

在 `docker-compose.yml` 同级创建 `.env` 文件：

```bash
# .env
POSTGRES_PASSWORD=mysecret
POSTGRES_DB=appdb
APP_PORT=5000
DEBUG=true
```

在 `docker-compose.yml` 中引用：

```yaml
services:
  web:
    ports:
      - "${APP_PORT}:5000"
    environment:
      DEBUG: "${DEBUG}"

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD: "${POSTGRES_PASSWORD}"
      POSTGRES_DB: "${POSTGRES_DB}"
```

> ⚠️ `.env` 文件不应提交到 Git（添加至 `.gitignore`）。

---

## 常用命令速查

| 命令 | 说明 |
|------|------|
| `docker compose up -d` | 后台启动全部服务 |
| `docker compose down` | 停止并移除容器和网络 |
| `docker compose ps` | 列出服务状态 |
| `docker compose logs -f` | 跟踪所有日志 |
| `docker compose logs -f web` | 跟踪指定服务日志 |
| `docker compose exec web sh` | 进入 web 容器 |
| `docker compose build` | 重新构建镜像 |
| `docker compose pull` | 拉取最新镜像 |
| `docker compose restart` | 重启所有服务 |
| `docker compose stop` | 停止（不删除） |
| `docker compose start` | 启动已停止的服务 |

---

## 常用 YAML 片段

### Node.js + MongoDB

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      MONGO_URI: mongodb://mongo:27017/appdb
    depends_on:
      - mongo
  mongo:
    image: mongo:7
    volumes:
      - mongodata:/data/db

volumes:
  mongodata:
```

### Nginx 反向代理 + 静态文件

```yaml
services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
      - ./static:/usr/share/nginx/html:ro
```
