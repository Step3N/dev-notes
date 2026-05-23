# Dockerfile 语法笔记

> 全平台通用。Dockerfile 是构建镜像的蓝图，每条指令生成一层。

---

## 指令速查

| 指令 | 说明 | 使用时机 |
|------|------|----------|
| `FROM` | 指定基础镜像 | **必须**，首选 Alpine / slim |
| `WORKDIR` | 设置工作目录 | 每个 RUN/CMD 前 |
| `COPY` | 复制文件到镜像 | 构建上下文中的文件 |
| `RUN` | 执行命令（构建时） | 安装依赖、编译 |
| `CMD` | 容器启动时的默认命令 | 可被 `docker run` 参数覆盖 |
| `ENTRYPOINT` | 容器入口点 | 不可被覆盖（除非 --entrypoint） |
| `EXPOSE` | 声明端口（文档用途） | 实际映射需 `-p` |
| `ENV` | 环境变量 | 运行时可用 |
| `ARG` | 构建时变量 | 仅构建时可用，不保留到镜像 |
| `VOLUME` | 声明挂载点 | 持久化数据 |
| `USER` | 指定运行用户 | 安全最佳实践 |
| `HEALTHCHECK` | 健康检查 | 生产环境必备 |
| `LABEL` | 元数据标签 | 版本、维护者等 |

---

## 最佳实践

### 1. 多阶段构建

```dockerfile
# 阶段一：编译
FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# 阶段二：运行（仅包含产物）
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 2. 利用构建缓存

```dockerfile
# 先复制依赖文件，再安装——依赖不变则缓存命中
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
# 后复制源码（频繁变化）
COPY . .
```

### 3. .dockerignore

创建 `.dockerignore` 文件排除无关文件：

```
.git/
__pycache__/
*.pyc
.env
node_modules/
dist/
.venv/
```

### 4. 最小化镜像

- 使用 `alpine` 或 `slim` 基础镜像
- 合并 `RUN` 指令：`RUN apt update && apt install -y pkg && rm -rf /var/lib/apt/lists/*`
- 使用 `--no-cache-dir`（pip）、`--no-cache`（apk）

### 5. 使用明确标签

```dockerfile
# ❌ 不推荐
FROM python:latest

# ✅ 推荐
FROM python:3.12-slim
```

### 6. 非 root 用户

```dockerfile
FROM node:20-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

---

## 完整示例：Python Flask 应用

```dockerfile
FROM python:3.12-slim

WORKDIR /app

# 安装系统依赖
RUN apt update && apt install -y --no-install-recommends \
    gcc \
    && rm -rf /var/lib/apt/lists/*

# 安装 Python 依赖
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 复制源码
COPY . .

# 非 root 用户
RUN adduser --disabled-password --gecos "" appuser
USER appuser

EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```

对应的 `requirements.txt`：

```
flask==3.1.*
gunicorn==23.*
```

---

## 完整示例：Node.js Express 应用

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production
COPY . .

FROM node:20-alpine
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app .

USER appuser
EXPOSE 3000

CMD ["node", "server.js"]
```

---

## 常用基础镜像一览

| 镜像 | 大小 | 用途 |
|------|------|------|
| `python:3.12-slim` | ~120MB | Python 应用 |
| `node:20-alpine` | ~120MB | Node.js 应用 |
| `nginx:alpine` | ~25MB | 静态文件服务 |
| `golang:1.22-alpine` | ~300MB | Go 编译 |
| `ubuntu:22.04` | ~80MB | 通用基础系统 |
| `alpine:3.19` | ~7MB | 最小基础系统 |
| `scratch` | 0 | 空镜像（静态编译） |

---

## 构建命令

```bash
# 构建
docker build -t my-app:1.0 .

# 指定 Dockerfile 路径
docker build -f docker/Dockerfile -t my-app .

# 构建时传参
docker build --build-arg VERSION=1.0 -t my-app .
```

> `-t` 指定标签，`.` 是构建上下文路径。
