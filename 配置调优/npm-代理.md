# npm 代理配置

## 设置代理

```bash
npm config set proxy http://127.0.0.1:7890
npm config set https-proxy http://127.0.0.1:7890
```

设置不代理的地址（本地网络不走代理）：

```bash
npm config set noproxy "localhost,127.0.0.1,.local"
```

---

## 查看代理配置

```bash
npm config get proxy
npm config get https-proxy
npm config get noproxy
```

查看所有配置：

```bash
npm config list
```

查看更详细的信息（包括从环境变量继承的配置）：

```bash
npm config list -l
```

---

## 删除代理配置

```bash
npm config delete proxy
npm config delete https-proxy
```

---

## 环境变量方式

npm 也读取系统环境变量，与 pip 共用：

```bash
# Linux / macOS
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
export no_proxy=localhost,127.0.0.1,.local
```

```powershell
# Windows PowerShell
$env:HTTP_PROXY="http://127.0.0.1:7890"
$env:HTTPS_PROXY="http://127.0.0.1:7890"
```

> npm 优先读大写 `HTTP_PROXY` 而不是小写 `http_proxy`。建议在环境中两者都设。

---

## 配合镜像源使用（中国大陆）

代理 + 淘宝源，双重加速：

```bash
npm config set registry https://registry.npmmirror.com
```

单次使用（不修改配置）：

```bash
npm install express --registry https://registry.npmmirror.com \
  --proxy http://127.0.0.1:7890
```

查看当前 registry：

```bash
npm config get registry
```

---

## Yarn 代理配置

```bash
yarn config set proxy http://127.0.0.1:7890
yarn config set https-proxy http://127.0.0.1:7890
```

查看：

```bash
yarn config get proxy
```

---

## pnpm 代理配置

```bash
pnpm config set proxy http://127.0.0.1:7890
pnpm config set https-proxy http://127.0.0.1:7890
```

查看：

```bash
pnpm config get proxy
```

---

## 验证代理是否生效

```bash
# 查看 proxychains 或 tcpdump 确认请求走了代理
npm install --prefer-online express

# verbose 模式可以看到请求详情
npm install --loglevel verbose express
```

---

## 常见问题

### `ECONNREFOUND` — 连接被拒绝

- 确认代理软件正在运行
- 确认端口正确

### `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`

代理证书导致 SSL 错误：

```bash
# 临时关闭 SSL 验证（不推荐长期使用）
npm config set strict-ssl false
```

建议通过 `NODE_EXTRA_CA_CERTS` 添加代理的 CA 证书。

### `ERR_INVALID_ARG_VALUE`（Windows）

Windows 上某些代理字符串含特殊字符，用引号包裹：

```powershell
npm config set proxy "http://user:password@proxy:port"
```

---

## 平台差异

| 操作 | **macOS** / **Linux** | **Windows** |
|------|----------------------|-------------|
| npm config 文件位置 | `~/.npmrc` | `%USERPROFILE%\.npmrc` |
| 环境变量 | 通用 | 通用 |
| 命令语法 | 通用 | 通用 |
