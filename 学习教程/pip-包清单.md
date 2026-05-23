# 常用 pip 包清单

## 开发工具

```bash
pip install ruff          # 极速 Python linter + formatter，替代 flake8 + isort + black
pip install mypy          # 静态类型检查，提前发现类型错误
pip install pytest        # 最流行的测试框架，简洁且插件生态丰富
pip install ipython       # 增强交互式 Python shell，支持 Tab 补全、语法高亮
pip install pre-commit    # Git hook 管理，自动在提交前运行 lint/test
```

> **ruff** 强烈推荐：比现有的 Flake8 + isort + Black 快 10-100 倍，且规则兼容。

---

## Web 框架

```bash
pip install flask         # 轻量微框架，适合 API 和小型应用
pip install fastapi       # 现代高性能异步框架，自动生成 OpenAPI 文档
pip install django        # 全栈框架，"开箱即用"，适合大型 Web 应用
```

> 小型 API 选 FastAPI，完整网站选 Django，微服务选 Flask。

---

## 数据处理

```bash
pip install numpy         # 数值计算基础库，N 维数组、线性代数、随机数
pip install pandas        # 表格数据处理，类似 Excel + SQL 的体验
pip install jupyter       # 交互式 Notebook，数据分析与可视化的首选
pip install matplotlib    # 最经典的 2D 绘图库
pip install scipy         # 科学计算，优化、插值、统计、信号处理
```

---

## CLI 工具

```bash
pip install click         # 命令行参数解析，装饰器风格，简洁优雅
pip install typer         # 基于 Click + 类型注解的 CLI 框架，代码更少
pip install rich          # 终端富文本渲染，表格、进度条、语法高亮
pip install httpx         # 现代 HTTP 客户端，支持同步/异步、HTTP/2
pip install requests      # 经典 HTTP 库（httpx 的现代替代，但 requests 生态更成熟）
```

> **typer + rich** 组合：快速构建漂亮的 CLI 应用。**httpx** 逐渐替代 requests。

---

## 工具库

```bash
pip install python-dotenv # 从 .env 文件加载环境变量，管理配置
pip install pydantic      # 数据校验与设置管理，基于类型注解（FastAPI 核心依赖）
pip install loguru        # 优雅的日志库，比 logging 好用 10 倍
pip install tqdm          # 进度条，循环加一行就有进度显示
pip install arrow         # 友好的人性化时间日期库，替代 datetime
pip install pendulum      # 时区感知的日期时间库，比 arrow 更精确
```

---

## 异步 / 性能

```bash
pip install aiohttp       # 异步 HTTP 客户端/服务端
pip install asyncio       # Python 内置，但注意第三方异步驱动（如 asyncpg, aiomysql）
pip install uvloop        # 替代 asyncio 事件循环，提升 2-4 倍性能
pip install cython        # 将 Python 编译为 C 扩展，提升性能
```

---

## 数据库

```bash
pip install sqlalchemy    # ORM + SQL 工具包，支持多种数据库
pip install alembic       # 数据库迁移管理（配合 SQLAlchemy）
pip install psycopg2-binary  # PostgreSQL 驱动
pip install pymysql       # MySQL 驱动
pip install redis         # Redis 缓存/消息队列客户端
pip install motor         # MongoDB 异步驱动
```

---

## DevOps / 部署

```bash
pip install invoke        # 任务运行器，用 Python 写部署脚本替代 Makefile
pip install fabric        # 远程服务器自动化部署
pip install supervisor    # 进程管理，确保应用持续运行
pip install gunicorn      # WSGI HTTP 服务器，部署 Flask/Django
pip install uvicorn       # ASGI HTTP 服务器，部署 FastAPI
```

---

## 一次性批量安装（常用组合）

```bash
# 开发环境
pip install ruff mypy pytest ipython pre-commit

# Web 开发
pip install fastapi uvicorn sqlalchemy alembic pydantic python-dotenv

# 数据科学
pip install numpy pandas jupyter matplotlib scipy

# CLI 工具
pip install typer rich httpx click
```
