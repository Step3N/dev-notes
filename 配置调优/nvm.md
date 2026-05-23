# nvm — Node Version Manager

Node.js 多版本管理工具。一台机器同时装多个 Node 版本，随时切换。

---

## 📦 安装

### 🍎 macOS / 🐧 Linux

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

装完后重启终端，或手动加载：

```bash
source ~/.zshrc   # zsh（macOS 默认、多数 Linux）
# 或
source ~/.bashrc  # bash
```

验证：

```bash
nvm --version
# 输出示例: 0.40.1
```

### 🪟 Windows

安装 **nvm-windows**（注意：与 nvm-sh/nvm 不同项目）：

1. 访问 https://github.com/coreybutler/nvm-windows/releases
2. 下载 `nvm-setup.exe`
3. 双击安装（建议默认路径）

验证：

```powershell
nvm version
# 输出示例: 1.2.x
```

---

## ⚙️ 常用命令

| 操作 | macOS / Linux | Windows |
|------|--------------|---------|
| 安装指定版本 | `nvm install 22` | `nvm install 22` |
| 安装最新 LTS | `nvm install --lts` | `nvm install lts` |
| 使用某版本 | `nvm use 22` | `nvm use 22` |
| 设置默认版本 | `nvm alias default 22` | `nvm alias default 22` |
| 列出已安装 | `nvm ls` | `nvm list` |
| 列出可安装 | `nvm ls-remote` | `nvm list available` |
| 卸载版本 | `nvm uninstall 18` | `nvm uninstall 18` |

快速实操：

```bash
# 安装 LTS 并设为默认
nvm install --lts
nvm alias default 'lts/*'
node --version

# 安装另一个版本试试
nvm install 20
nvm use 20
node --version

# 切回 LTS
nvm use --lts
```

---

## 📄 .nvmrc — 项目级版本锁定

在项目根目录创建 `.nvmrc` 文件：

```
22
```

或更精确：

```
22.14.0
```

然后进入项目目录：

```bash
nvm use
# 自动读取 .nvmrc 并切换版本
```

> 团队协作时，`.nvmrc` 确保所有人用同一 Node 版本。

### 配合 shell 自动加载

在 `~/.zshrc` 中添加（macOS/Linux）：

```bash
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")
    if [ "$nvmrc_node_version" != "N/A" ] && [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

之后 `cd` 进入有 `.nvmrc` 的目录自动切换 Node 版本。

---

## ⚠️ 常见问题

### `nvm: command not found`

装了但找不到命令，通常是 shell 配置未加载。

**macOS / Linux**：

```bash
# 检查 ~/.zshrc 或 ~/.bashrc 是否包含以下内容（安装脚本自动添加的）：
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# 手动加载
source ~/.zshrc
```

**Windows**：

- 重启终端
- 以管理员身份运行
- 检查环境变量 Path 是否包含 `C:\Users\<用户名>\AppData\Roaming\nvm`

### 卸载 nvm

**macOS / Linux**：

```bash
rm -rf "$NVM_DIR"
# 然后从 ~/.zshrc / ~/.bashrc 中删除 nvm 相关行
```

**Windows**：

```powershell
# 运行 nvm-windows 卸载程序
# 或手动删除目录和环境变量
```

### `nvm install` 编译失败（Linux）

缺少构建工具：

```bash
# Ubuntu / Debian
sudo apt install -y build-essential

# CentOS / Fedora
sudo dnf groupinstall "Development Tools"
```

---

## 🔗 参考

- nvm-sh/nvm (macOS/Linux): https://github.com/nvm-sh/nvm
- nvm-windows: https://github.com/coreybutler/nvm-windows
