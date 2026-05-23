# pip 代理配置

## 三种配置方式（任选其一）

---

### 方式一：全局配置文件

**Linux / macOS** — `~/.pip/pip.conf`：

```ini
[global]
proxy = http://127.0.0.1:7890
```

**Windows** — `%APPDATA%\pip\pip.ini`：

```ini
[global]
proxy = http://127.0.0.1:7890
```

创建配置目录：

```bash
# Linux / macOS
mkdir -p ~/.pip && cat > ~/.pip/pip.conf <<'EOF'
[global]
proxy = http://127.0.0.1:7890
EOF
```

```powershell
# Windows PowerShell
New-Item -ItemType Directory -Force -Path "$env:APPDATA\pip"
@"
[global]
proxy = http://127.0.0.1:7890
"@ | Out-File -Encoding utf8 "$env:APPDATA\pip\pip.ini"
```

---

### 方式二：环境变量

**Linux / macOS**（添加到 `~/.zshrc` 或 `~/.bashrc`）：

```bash
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
export no_proxy=localhost,127.0.0.1,.local
```

**Windows PowerShell**（添加到 `$PROFILE`）：

```powershell
$env:http_proxy="http://127.0.0.1:7890"
$env:https_proxy="http://127.0.0.1:7890"
$env:no_proxy="localhost,127.0.0.1"
```

**Windows CMD**：

```cmd
set http_proxy=http://127.0.0.1:7890
set https_proxy=http://127.0.0.1:7890
```

> pip 同时读取小写 `http_proxy` 和大写 `HTTP_PROXY`，建议都设置。

---

### 方式三：单次使用（不修改配置）

```bash
pip install requests --proxy http://127.0.0.1:7890
```

pipenv 同样支持：

```bash
pipenv install --proxy http://127.0.0.1:7890
```

---

## 配合镜像源使用（中国大陆）

代理 + 清华源，双重加速：

```bash
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple \
  --proxy http://127.0.0.1:7890
```

写入配置文件：

```ini
[global]
proxy = http://127.0.0.1:7890
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

---

## 验证代理是否生效

```bash
pip config list
```

查看当前环境变量中的代理：

```bash
# Linux / macOS
echo $http_proxy

# Windows PowerShell
echo $env:http_proxy
```

抓包验证（确保 pip 走了代理端口）：

```bash
# 先确定代理端口在监听
lsof -i :7890
```

---

## 常见问题

### `ProxyError` / `Connection refused`

- 确认代理客户端正在运行
- 确认端口号正确
- 检查代理协议（http 还是 socks5？pip 只支持 http 代理）

### 忽略代理安装本地包

```bash
pip install --no-proxy /path/to/package.whl
```

### macOS 证书问题

```bash
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org <package>
```

---

## 平台差异

| 配置方式 | **macOS** / **Linux** | **Windows** |
|---------|----------------------|-------------|
| 配置文件 | `~/.pip/pip.conf` | `%APPDATA%\pip\pip.ini` |
| 环境变量 | `~/.zshrc` / `~/.bashrc` | `$PROFILE` |
| 单次 `--proxy` | 通用 | 通用 |
