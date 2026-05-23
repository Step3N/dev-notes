# Linux — 开发环境优化

拿到一台新 Linux 机器后，按顺序跑完这些配置，开发体验从及格到舒适。

> 以下命令基于 Ubuntu/Debian，其他发行版用对应的包管理器替换（参见 `apt-dnf-pacman.md`）。

---

## 📦 安装基础开发包

```bash
# Debian/Ubuntu
sudo apt update && sudo apt upgrade -y
sudo apt install -y \
  build-essential \
  curl \
  wget \
  git \
  vim \
  htop \
  net-tools \
  ca-certificates \
  gnupg \
  lsb-release \
  unzip \
  tree \
  jq \
  ripgrep \
  tmux

# Fedora/RHEL
sudo dnf groupinstall "Development Tools"
sudo dnf install -y curl wget git vim htop net-tools unzip tree jq ripgrep tmux

# Arch
sudo pacman -S --needed base-devel curl wget git vim htop net-tools unzip tree jq ripgrep tmux
```

---

## 🐚 安装 Zsh & 设为默认 Shell

```bash
# 安装 Zsh
sudo apt install -y zsh

# 设置为默认 Shell
chsh -s $(which zsh)

# 退出重新登录，或直接切换
zsh
```

> 注意：`chsh` 需要输入密码，且用户必须在 `/etc/shells` 中有 zsh 的路径。

安装 Oh My Zsh（可选但推荐）：

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 推荐插件
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# 在 ~/.zshrc 中启用插件
# plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

---

## 💾 交换分区配置

RAM < 8GB 时建议加大 swap：

```bash
# 查看当前 swap
swapon --show
free -h

# 创建 4GB swap 文件
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 持久化到 /etc/fstab
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# 调整 swappiness（越低越少用 swap，推荐 10-60）
sudo sysctl vm.swappiness=10
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.d/99-swap.conf
```

---

## 🔩 文件描述符限制

开发工具（如 `file watchers`、`node --watch`、`nginx`）可能超过默认 1024 限制：

```bash
# 临时调整（当前 shell）
ulimit -n 65536

# 永久调整
sudo tee -a /etc/security/limits.conf << 'EOF'
*                soft    nofile          65536
*                hard    nofile          65536
root             soft    nofile          65536
root             hard    nofile          65536
EOF

# 同时调整 systemd 用户的限制（如果使用 systemd user units）
mkdir -p ~/.config/systemd/user.d/
cat > ~/.config/systemd/user.d/limits.conf << 'EOF'
[Manager]
DefaultLimitNOFILE=65536
EOF
```

> 修改后需重新登录生效。验证：`ulimit -n`。

---

## 🧹 禁用不必要的服务

```bash
# 列出所有服务
systemctl list-unit-files --type=service --state=enabled

# 常见可选禁用的服务（如果你不需要打印机）
sudo systemctl disable --now cups.service          # 打印服务
sudo systemctl disable --now avahi-daemon.service   # mDNS 广播
sudo systemctl disable --now bluetooth.service      # 如果你不用蓝牙
sudo systemctl disable --now whoopsie.service       # Ubuntu 错误上报
sudo systemctl disable --now snapd.service          # 如果不用 snap
sudo systemctl disable --now ModemManager.service   # 非笔记本不需要
```

> 每次禁用后建议确认：`systemctl status <服务名>` 显示 `disabled`。

---

## 🔥 防火墙基础配置

```bash
# 安装 ufw
sudo apt install -y ufw

# 默认策略：入站拒绝，出站允许
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 开放 SSH
sudo ufw allow ssh
# sudo ufw allow 22/tcp

# 开放 HTTP/HTTPS（如果需要）
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# 开启防火墙
sudo ufw enable

# 查看状态
sudo ufw status verbose
```

---

## 🔄 自动安全更新

```bash
sudo apt install -y unattended-upgrades

# 配置自动更新
sudo dpkg-reconfigure --priority=low unattended-upgrades
# 选择 Yes

# 或直接编辑配置
sudo tee -a /etc/apt/apt.conf.d/20auto-upgrades << 'EOF'
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
EOF

# 验证配置
sudo unattended-upgrades --dry-run --debug
```

---

## 👀 内核参数调优 — inotify

文件监听工具（`nodemon`、`webpack --watch`、`watchexec` 等）默认监控数量有限：

```bash
# 查看当前限制
cat /proc/sys/fs/inotify/max_user_watches
# 默认: 8192

# 临时调整
sudo sysctl fs.inotify.max_user_watches=524288

# 永久调整
echo 'fs.inotify.max_user_watches=524288' | sudo tee -a /etc/sysctl.d/99-inotify.conf

# 生效
sudo sysctl -p /etc/sysctl.d/99-inotify.conf
```

其他实用的内核参数：

```bash
# 提升网络性能
echo 'net.core.somaxconn = 1024' | sudo tee -a /etc/sysctl.d/99-network.conf
echo 'net.ipv4.tcp_tw_reuse = 1' | sudo tee -a /etc/sysctl.d/99-network.conf
echo 'net.ipv4.tcp_fin_timeout = 15' | sudo tee -a /etc/sysctl.d/99-network.conf

# 允许普通用户监听低端口（开发时需要）
echo 'net.ipv4.ip_unprivileged_port_start = 80' | sudo tee -a /etc/sysctl.d/99-unpriv-ports.conf

sudo sysctl --system
```

---

## ✅ 验证清单

| 项目 | 验证命令 | 期望结果 |
|------|---------|---------|
| 开发工具 | `gcc --version; git --version` | 正常版本号 |
| Zsh | `echo $SHELL` | `/usr/bin/zsh` |
| Swap | `free -h` | swap 行有值 |
| 文件描述符 | `ulimit -n` | 65536 |
| UFW | `sudo ufw status` | `Status: active` |
| 自动更新 | `systemctl status unattended-upgrades` | `Active: active` |
| inotify | `cat /proc/sys/fs/inotify/max_user_watches` | 524288 |

---

## 🔗 参考

- https://wiki.archlinux.org/title/Security
- https://wiki.debian.org/UnattendedUpgrades
- https://www.kernel.org/doc/Documentation/sysctl/
