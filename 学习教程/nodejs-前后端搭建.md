# [链路] Node.js 前后端项目搭建

> 从零开始，搭建一个完整的前后端分离项目，跑通开发到部署

**适用平台**：macOS / Windows / Linux
**预估耗时**：40-60 分钟
**前置条件**：
- 有基本命令行使用经验
- 不需要有 Node.js 经验

---

## 链路总览

| 步骤 | 内容 | 耗时 |
|------|------|------|
| Step 1 | 安装 Node.js (via nvm) | 5 min |
| Step 2 | 配置 npm 镜像源 | 1 min |
| Step 3 | 创建前端项目 (Vite + React + TypeScript) | 5 min |
| Step 4 | 创建后端项目 (Express + TypeScript) | 10 min |
| Step 5 | 连接 PostgreSQL 数据库 | 10 min |
| Step 6 | 配置环境变量 | 3 min |
| Step 7 | Docker 打包部署 | 10 min |
| Final | 最终验证 | 2 min |

---

## Step 1: 安装 Node.js (via nvm)

nvm 是 Node.js 版本管理工具，让你可以随时切换 Node 版本，避免权限问题。

```bash
# macOS / Linux — 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# 重启终端，或执行以下命令立即生效
source ~/.zshrc   # macOS (zsh)
source ~/.bashrc  # Linux (bash)
```

```powershell
# Windows — 从 https://github.com/coreybutler/nvm-windows/releases 下载 nvm-setup.exe
# 安装后打开新的 PowerShell 或 CMD
```

验证 nvm：

```bash
nvm --version  # 预期: 0.40.1
```

安装并使用 Node.js LTS：

```bash
nvm install --lts    # 安装最新 LTS 版
nvm use --lts        # 使用 LTS 版
nvm alias default lts/*   # 设为默认版本（新终端自动使用）
```

验证 Node.js 和 npm：

```bash
node --version   # 预期: v22.x.x 或 v20.x.x
npm --version    # 预期: 10.x.x
```

> 📖 详见 [nvm 版本管理](../配置调优/nvm.md) / [Node.js 安装](../安装软件/nodejs.md)

---

## Step 2: 配置 npm 镜像源 (国内用户)

国内网络访问 npm 官方源速度慢，推荐切换到 npmmirror：

```bash
npm config set registry https://registry.npmmirror.com
npm config get registry  # 验证: 输出 https://registry.npmmirror.com
```

如果后续需要切换回官方源：

```bash
npm config set registry https://registry.npmjs.org
```

### 可选：使用 pnpm（更快，节省磁盘空间）

```bash
npm install -g pnpm
pnpm --version  # 预期: 9.x.x
```

pnpm 的优点：
- 通过硬链接共享依赖，多个项目共用同一份包
- 安装速度比 npm 快 2-3 倍
- 兼容 npm 的所有命令（`pnpm add` = `npm install`）

> 📖 详见 [npm 换源](../配置调优/npm-换源.md)

---

## Step 3: 创建前端项目 (Vite + React + TypeScript)

使用 Vite 官方脚手架创建项目：

```bash
npm create vite@latest frontend -- --template react-ts
cd frontend
npm install
```

项目结构：

```
frontend/
├── src/            # 源代码
│   ├── App.tsx     # 主组件
│   ├── main.tsx    # 入口文件
│   └── vite-env.d.ts
├── index.html      # HTML 模板
├── package.json
├── tsconfig.json
└── vite.config.ts  # Vite 配置
```

启动开发服务器：

```bash
npm run dev
# 终端显示: Local: http://localhost:5173
```

浏览器打开 http://localhost:5173 应看到 Vite + React 欢迎页。

### 添加 API 请求示例

修改 `src/App.tsx` 添加后端调用：

```typescript
import { useEffect, useState } from 'react';

function App() {
  const [message, setMessage] = useState('');

  useEffect(() => {
    fetch('http://localhost:3001/api/hello')
      .then((res) => res.json())
      .then((data) => setMessage(data.message));
  }, []);

  return (
    <div>
      <h1>Vite + React + TypeScript</h1>
      <p>后端消息: {message || '等待连接...'}</p>
    </div>
  );
}

export default App;
```

---

## Step 4: 创建后端项目 (Express + TypeScript)

在项目根目录下创建 `backend` 文件夹：

```bash
cd ..  # 回到项目根目录
mkdir backend && cd backend
npm init -y
```

安装生产依赖：

```bash
npm install express cors dotenv
```

安装开发依赖：

```bash
npm install -D typescript @types/node @types/express @types/cors ts-node nodemon
```

初始化 TypeScript 配置：

```bash
npx tsc --init
```

编辑 `tsconfig.json` 确保以下关键字段：

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

创建入口文件 `backend/src/index.ts`：

```typescript
import express from 'express';
import cors from 'cors';
import dotenv from 'dotenv';

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3001;

app.use(cors());
app.use(express.json());

app.get('/api/hello', (_req, res) => {
  res.json({ message: 'Hello from Express!' });
});

app.get('/api/health', (_req, res) => {
  res.json({ status: 'ok', timestamp: new Date().toISOString() });
});

app.listen(PORT, () => {
  console.log(`Backend running on http://localhost:${PORT}`);
});
```

编辑 `backend/package.json`，添加 scripts：

```json
{
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "typecheck": "tsc --noEmit"
  }
}
```

启动后端：

```bash
npm run dev
# 预期输出: Backend running on http://localhost:3001
```

验证：

```bash
curl http://localhost:3001/api/hello
# 预期: {"message":"Hello from Express!"}
```

浏览器打开 http://localhost:3001/api/hello 也应看到 JSON 响应。

---

## Step 5: 连接数据库 (PostgreSQL)

### 安装 PostgreSQL

```bash
# macOS
brew install postgresql@16
brew services start postgresql@16

# Windows — 从 https://www.enterprisedb.com/downloads/postgres-postgresql-downloads 下载安装包
# 安装过程中设置密码（记住它！）

# Linux (Ubuntu/Debian)
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### 创建数据库

```bash
# macOS / Linux
psql -U postgres

# Windows — 在开始菜单打开 "SQL Shell (psql)"，按提示输入
```

在 psql 交互中执行：

```sql
CREATE DATABASE myapp;
\q
```

### 安装 Node.js PostgreSQL 客户端

```bash
npm install pg @types/pg
```

创建 `backend/src/db.ts`：

```typescript
import { Pool } from 'pg';

const pool = new Pool({
  user: process.env.DB_USER || 'postgres',
  host: process.env.DB_HOST || 'localhost',
  database: process.env.DB_NAME || 'myapp',
  password: process.env.DB_PASSWORD || '',
  port: parseInt(process.env.DB_PORT || '5432'),
});

export async function query(text: string, params?: any[]) {
  const start = Date.now();
  const result = await pool.query(text, params);
  const duration = Date.now() - start;
  console.log(`Query executed in ${duration}ms, rows: ${result.rowCount}`);
  return result;
}

export default pool;
```

在 `backend/src/index.ts` 中添加测试路由：

```typescript
import pool from './db';

app.get('/api/db-check', async (_req, res) => {
  try {
    const result = await pool.query('SELECT NOW() AS current_time');
    res.json({ connected: true, time: result.rows[0].current_time });
  } catch (err) {
    res.status(500).json({ connected: false, error: String(err) });
  }
});
```

重启后端后验证：

```bash
curl http://localhost:3001/api/db-check
# 预期: {"connected":true,"time":"2026-05-23T12:00:00.000Z"}
```

> 📖 详见 [数据库本地环境](../安装软件/database.md)

---

## Step 6: 配置环境变量

在 `backend/` 根目录创建 `.env` 文件：

```
PORT=3001
DB_USER=postgres
DB_PASSWORD=
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
```

代码中已通过 `dotenv.config()` 自动加载（见 `src/index.ts` 顶部）。

**安全提醒**：
- `.env` 文件不要提交到 Git（已在 `.gitignore` 中添加）
- 生产环境通过 Docker / CI 变量 / 密钥管理服务注入环境变量

创建 `backend/.gitignore`：

```
node_modules/
dist/
.env
*.log
```

> 📖 详见 [环境变量管理](../配置调优/环境变量管理.md)

---

## Step 7: Docker 打包部署

### 后端 Dockerfile

创建 `backend/Dockerfile`：

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./
RUN npm ci --only=production
EXPOSE 3001
CMD ["node", "dist/index.js"]
```

### docker-compose（项目根目录）

创建 `docker-compose.yml`：

```yaml
version: '3.8'

services:
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "3001:3001"
    environment:
      - PORT=3001
      - DB_HOST=db
      - DB_PORT=5432
      - DB_USER=postgres
      - DB_PASSWORD=password
      - DB_NAME=myapp
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    restart: unless-stopped

volumes:
  pgdata:
```

构建并启动：

```bash
docker compose up -d
```

验证：

```bash
docker compose ps
# 预期: 两个服务状态均为 "Up"

curl http://localhost:3001/api/health
# 预期: {"status":"ok","timestamp":"..."}

curl http://localhost:3001/api/db-check
# 预期: {"connected":true,"time":"..."}
```

停止：

```bash
docker compose down
# 加 -v 会删除数据卷（慎用）
docker compose down -v
```

---

## 最终验证

从零开始的完整验证流程：

```bash
# 1. 启动后端
cd backend && npm run dev
# 输出: Backend running on http://localhost:3001

# 2. 新终端，启动前端
cd frontend && npm run dev
# 输出: Local: http://localhost:5173

# 3. 验证后端 API
curl http://localhost:3001/api/hello
# 预期: {"message":"Hello from Express!"}

# 4. 验证数据库连接
curl http://localhost:3001/api/db-check
# 预期: {"connected":true,"time":"..."}

# 5. 浏览器访问 http://localhost:5173
# 预期: 页面显示 "后端消息: Hello from Express!"

# 6. Docker 部署
cd .. && docker compose up -d
docker compose ps
# 预期: backend 和 db 都在运行
```

---

## 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| `port 3001 already in use` | 端口被占用 | `lsof -i :3001` → `kill -9 <PID>`（macOS/Linux）<br>Windows: `netstat -ano | findstr :3001` → `taskkill /PID <PID> /F` |
| `ts-node: command not found` | 没装 ts-node | `npm install -D ts-node` 后在 backend 目录运行 |
| `psql: command not found` | PostgreSQL 客户端未安装或不在 PATH | macOS: `brew link --overwrite postgresql@16`<br>检查 PostgreSQL 安装是否完整 |
| `ECONNREFUSED :5432` | PostgreSQL 服务未启动 | macOS: `brew services start postgresql@16`<br>Linux: `sudo systemctl start postgresql` |
| Docker 容器连不上数据库 | 容器网络不通或数据库未就绪 | 检查 docker-compose 中 `depends_on` 和 `healthcheck` 配置<br>查看日志: `docker compose logs db` |
| npm install 报权限错误 | 系统目录权限问题 | 使用 nvm 安装 Node（不需要 sudo），确保在项目目录内操作 |
| `Cannot find module '@types/node'` | 类型声明未安装 | `npm install -D @types/node` |

---

## 下一步

- [Docker 项目运行](./docker-项目运行.md) — 深入学习容器编排
- [Python 深度学习环境搭建](./python-深度学习环境搭建.md) — 搭建 AI 训练环境
- [nginx 配置](../学习教程/nginx.md) — 反向代理与负载均衡
- [yarn/pnpm 配置](../学习教程/yarn-pnpm.md) — 包管理器对比与选择
