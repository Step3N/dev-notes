# 本地数据库环境安装

> 涵盖 MySQL、PostgreSQL、Redis，三平台安装方式。

---

## MySQL

### macOS

```bash
brew install mysql
```

启动服务：

```bash
brew services start mysql@8.4
```

验证：

```bash
mysql --version
```

### Windows

```powershell
winget install MySQL.Server
```

或从 [MySQL Installer](https://dev.mysql.com/downloads/installer/) 下载。

安装后 MySQL 自动注册为 Windows 服务。手动启动：

```powershell
net start MySQL80
```

验证：

```powershell
mysql --version
```

### Linux

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install -y mysql-server

# 启动
sudo systemctl enable --now mysql
```

验证：

```bash
mysql --version
```

### 基础安全配置

```bash
# macOS / Linux
sudo mysql_secure_installation
```

按提示设置 root 密码、移除匿名用户、禁止远程 root 登录。

### 连接与测试

```bash
# 方式一：直接连接
mysql -u root -p

# 方式二：使用 socket（Linux 常见）
sudo mysql
```

常用操作：

```sql
CREATE DATABASE mydb;
USE mydb;
CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(100));
INSERT INTO users (name) VALUES ('Alice');
SELECT * FROM users;
```

---

## PostgreSQL

### macOS

```bash
brew install postgresql@16
```

添加到 PATH（brew 会提示，类似如下）：

```bash
echo 'export PATH="/opt/homebrew/opt/postgresql@16/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

启动：

```bash
brew services start postgresql@16
```

验证：

```bash
psql --version
```

### Windows

```powershell
# 方式一：winget
winget install PostgreSQL.PostgreSQL.16

# 方式二：EDB Installer（推荐）
# 从 https://www.enterprisedb.com/downloads/postgres-postgresql-downloads 下载
```

winget 安装后，在开始菜单打开 "SQL Shell (psql)"，或手动添加到 PATH：

```powershell
# 添加到 PATH（根据版本调整路径）
$env:Path += ";C:\Program Files\PostgreSQL\16\bin"
```

### Linux

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install -y postgresql postgresql-client

# 启动
sudo systemctl enable --now postgresql
```

### 创建用户与数据库

```bash
# 连接（Linux 默认用 postgres 系统用户）
sudo -u postgres psql

# macOS（当前系统用户即为 PostgreSQL 超级用户）
psql postgres
```

```sql
-- 创建用户
CREATE USER myuser WITH PASSWORD 'mypassword';

-- 创建数据库并授权
CREATE DATABASE mydb OWNER myuser;
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;

-- 连接新数据库
\c mydb

-- 建表测试
CREATE TABLE notes (
  id SERIAL PRIMARY KEY,
  title VARCHAR(200),
  body TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

INSERT INTO notes (title, body) VALUES ('Hello', 'World');
SELECT * FROM notes;

-- 退出
\q
```

### 连接工具

```bash
# psql 直接连接
psql -h localhost -U myuser -d mydb -W
```

---

## Redis

### macOS

```bash
brew install redis
```

启动：

```bash
brew services start redis
```

### Windows

Windows 上 Redis 官方不提供原生安装，推荐以下方式：

**方式一：WSL2**

```powershell
wsl -d Ubuntu -- sudo apt install -y redis-server
wsl -d Ubuntu -- sudo service redis-server start
```

**方式二：winget（非官方封装）**

```powershell
winget install Redis.Redis
```

**方式三：Docker（通用）**

```bash
docker run -d --name redis -p 6379:6379 redis:7-alpine
```

### Linux

```bash
# Ubuntu / Debian
sudo apt update && sudo apt install -y redis-server

# 启动
sudo systemctl enable --now redis-server
```

### 验证 Redis 运行

```bash
redis-cli ping
```

预期输出：`PONG`

```bash
# 更多验证
redis-cli
```

```redis
set mykey "hello"
get mykey
# 输出: "hello"

info server
```

### 基本使用

```bash
# 查看状态
redis-cli ping
redis-cli info | grep version

# 交互模式
redis-cli
```

### 配置 Redis 密码（可选）

编辑配置文件：

```bash
# macOS / Linux
# 查找配置文件位置
redis-cli CONFIG GET dir

# 编辑
sudo vim /opt/homebrew/etc/redis.conf   # macOS
sudo vim /etc/redis/redis.conf          # Linux
```

找到 `# requirepass foobared` 去掉注释并修改密码：

```
requirepass yourpassword
```

重启：

```bash
brew services restart redis   # macOS
sudo systemctl restart redis  # Linux
```

验证密码：

```bash
redis-cli -a yourpassword
```

---

## 三平台命令速查

| 操作 | macOS | Windows | Linux |
|------|-------|---------|-------|
| **MySQL** | `brew install mysql` | `winget install MySQL.Server` | `apt install mysql-server` |
| 启动 MySQL | `brew services start mysql@8.4` | `net start MySQL80` | `systemctl start mysql` |
| 连接 MySQL | `mysql -u root -p` | `mysql -u root -p` | `sudo mysql` |
| **PostgreSQL** | `brew install postgresql@16` | `winget install PostgreSQL.PostgreSQL.16` | `apt install postgresql` |
| 启动 PG | `brew services start postgresql@16` | 自动注册服务 | `systemctl start postgresql` |
| 连接 PG | `psql postgres` | `psql -U postgres` | `sudo -u postgres psql` |
| **Redis** | `brew install redis` | `winget install Redis.Redis` | `apt install redis-server` |
| 启动 Redis | `brew services start redis` | WSL2 方式 | `systemctl start redis-server` |
| 测试 Redis | `redis-cli ping` → PONG | 同上 | `redis-cli ping` → PONG |
