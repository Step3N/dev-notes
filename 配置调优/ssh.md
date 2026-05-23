# SSH 配置管理

## 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

参数说明：
- `-t ed25519` — 指定密钥类型为 Ed25519（更安全、性能更好，**推荐**）
- `-t rsa -b 4096` — 如果服务器不支持 Ed25519，退而使用 RSA 4096 位
- `-C` — 注释，通常填邮箱，方便识别密钥归属

查看生成的密钥：

```bash
ls -la ~/.ssh/
```

## 密钥文件说明

| 文件 | 说明 | 权限 |
|------|------|------|
| `~/.ssh/id_ed25519` | **私钥**，绝不可泄露 | `600` |
| `~/.ssh/id_ed25519.pub` | **公钥**，可添加到服务器 | `644` |
| `~/.ssh/` | 目录本身 | `700` |

设置正确权限：

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

## ssh-agent 管理

启动 agent 并将密钥添加到内存：

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### macOS：将密钥持久化到钥匙串

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

在 `~/.ssh/config` 中加入：

```
Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519
```

### Linux：自动启动 ssh-agent

在 `~/.bashrc` 或 `~/.zshrc` 中加入：

```bash
if ! pgrep -u "$USER" ssh-agent > /dev/null 2>&1; then
    eval "$(ssh-agent -s)" > /dev/null
fi
```

## ~/.ssh/config 配置

配置文件路径：`~/.ssh/config`，支持别名、端口、密钥等。

### 基础示例

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519

Host my-server
  HostName 192.168.1.100
  User root
  Port 2222
  IdentityFile ~/.ssh/id_ed25519
```

配置后可直接 `ssh my-server`，效果等同 `ssh root@192.168.1.100 -p 2222`。

### 跳板机（Jump Host）

```
Host bastion
  HostName jump.example.com
  User admin
  IdentityFile ~/.ssh/id_ed25519

Host internal-server
  HostName 10.0.1.50
  User ubuntu
  ProxyJump bastion
```

### 多密钥管理：不同 Host 用不同密钥

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work

Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
```

## 复制公钥到服务器

### 方法一：ssh-copy-id（推荐）

```bash
ssh-copy-id user@host
```

Linux 一般自带，macOS 需要 `brew install ssh-copy-id`。

### 方法二：手动追加

```bash
cat ~/.ssh/id_ed25519.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && chmod 700 ~/.ssh"
```

### 验证

```bash
ssh user@host "echo 连接成功"
```

## SSH 隧道（端口转发）

### 本地转发：访问远程内网服务

```bash
ssh -L 8080:localhost:80 user@host
```

将远程服务器的 80 端口映射到本机 8080，访问 `http://localhost:8080` 即可。

### 远程转发：让远程访问本地服务

```bash
ssh -R 9000:localhost:3000 user@host
```

远程服务器的 9000 端口流量转发到本机 3000 端口。

### 动态 SOCKS 代理

```bash
ssh -D 1080 user@host
```

配合浏览器设置 SOCKS5 代理 `localhost:1080`，所有流量通过远程服务器转发。

### 持久化隧道（autossh）

```bash
brew install autossh   # macOS
apt install autossh    # Linux

autossh -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -L 8080:localhost:80 user@host
```

## 安全最佳实践

1. **私钥权限**：永远设置为 `600`，不要分享
2. **禁用服务器密码登录**：
   ```bash
   sudo sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
   sudo sed -i 's/^PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
   sudo systemctl restart sshd
   ```
3. **禁用 root 直接登录**：`PermitRootLogin no`
4. **使用 SSH Config 管理连接**：减少手动输入，防止错误
5. **定期轮换密钥**：重新生成并更新 `authorized_keys`

## 快速验证清单

```bash
# 1. 密钥权限
stat -f "%A" ~/.ssh/id_ed25519     # macOS：应为 600
stat -c "%a" ~/.ssh/id_ed25519     # Linux：应为 600

# 2. 连接测试
ssh -T git@github.com               # GitHub 测试
ssh user@host "hostname"            # 服务器连通性

# 3. 查看已加载密钥
ssh-add -l

# 4. 测试配置语法（如果有的话）
ssh -G my-server                    # 打印解析后的配置
```

> **平台差异**：macOS/Linux 原生支持。Windows 使用 OpenSSH for Windows（Win 10 1809+ 自带）或 WSL。配置文件语法全平台通用。
