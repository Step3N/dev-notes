# Git 代理配置

## 什么时候需要 Git 代理

- 公司防火墙限制外网访问
- 中国大陆 GFW 导致 `github.com` 连接缓慢或失败
- 需要走 VPN 隧道访问内网 Git 仓库
- SSH 端口 22 被封，需要走 HTTPS 或 Socks 代理

---

## 全局 HTTP/HTTPS 代理

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

> 7890 是 Clash / Sing-box 等代理工具的默认端口，按实际情况替换。

---

## Socks5 代理

```bash
git config --global http.proxy socks5://127.0.0.1:1080
```

> Socks5 代理不区分 HTTP/HTTPS，一条即可。1080 是 Shadowsocks / V2Ray 的常见端口。

---

## 按域名配置代理

只对 `github.com` 走代理，其他仓库不走：

```bash
git config --global http.https://github.com.proxy http://127.0.0.1:7890
```

包含 `github.com` 和 `api.github.com`：

```bash
git config --global http.https://github.com.proxy http://127.0.0.1:7890
git config --global http.https://raw.githubusercontent.com.proxy http://127.0.0.1:7890
```

---

## SSH 代理

编辑 `~/.ssh/config`，让 SSH 协议的 Git 操作走 Socks5 代理：

```
Host github.com
  HostName github.com
  User git
  Port 22
  ProxyCommand nc -X connect -x 127.0.0.1:1080 %h %p
```

**Windows**（Git for Windows 自带 `connect` 工具）：

```
Host github.com
  HostName github.com
  User git
  Port 22
  ProxyCommand connect -S 127.0.0.1:1080 %h %p
```

---

## 查看当前代理设置

```bash
git config --global --get-regexp http.*
git config --global --get-regexp https.*
```

查看所有全局配置：

```bash
git config --global --list
```

---

## 删除代理配置

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy

# 删除按域名配置的代理
git config --global --unset http.https://github.com.proxy
```

---

## 常见问题

### SSL 证书错误

代理劫持 SSL 证书导致 `SSL certificate problem`：

```bash
git config --global http.sslverify false
```

> **安全警告**：仅临时使用，用完恢复 `git config --global http.sslverify true`，否则中间人攻击风险。

### 代理超时

```bash
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
```

### 验证代理是否生效

```bash
# 查看实际请求 IP
curl -I https://github.com
# 或使用 git 跟踪
GIT_CURL_VERBOSE=1 git clone https://github.com/...

# SSH 调试
ssh -vT git@github.com
```

---

## 平台差异

| 操作 | **macOS** / **Linux** | **Windows** |
|------|----------------------|-------------|
| SSH ProxyCommand | `nc -X connect -x ...` | `connect -S ...`（Git Bash） |
| 配置文件位置 | `~/.ssh/config` | `~/.ssh/config`（Git Bash 下） |
| 环境变量 | 通用 | 通用 |

---

## 优先级

命令行 > 按域名配置 > 全局配置 > 系统环境变量 (`http_proxy`)
