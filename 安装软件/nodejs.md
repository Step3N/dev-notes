# Node.js 安装

Node.js 是 JavaScript 运行时环境。以下覆盖所有平台。

---

## 版本选择：LTS vs Current

| 版本 | 说明 |
|------|------|
| **LTS** (长期支持) | 稳定、推荐生产环境。当前 LTS：22.x |
| **Current** (最新) | 含新特性但可能不稳定，适合尝鲜 |

> 新手/生产用 **LTS**，体验新语法用 **Current**。

---

## 🍎 macOS

### 方式一：Homebrew（推荐）

```bash
# 安装 LTS
brew install node

# 安装特定版本
brew install node@20
```

验证：

```bash
node --version
npm --version
```

> Homebrew 装的是 **最新版**。如需多版本切换，用 nvm（见 `nvm-版本管理.md`）。

### 方式二：官方 pkg 安装包

1. 访问 https://nodejs.org
2. 下载 **macOS .pkg**（LTS 或 Current）
3. 双击安装，一路下一步

验证：

```bash
node --version
```

> 安装在 `/usr/local/lib/node_modules`，自动写入 PATH。

---

## 🪟 Windows

### 方式一：winget

```powershell
# 安装 LTS
winget install OpenJS.NodeJS.LTS

# 安装 Current
winget install OpenJS.NodeJS
```

### 方式二：官方 .msi 安装包

1. 访问 https://nodejs.org
2. 下载 **Windows .msi**
3. 双击安装，**勾选 "Add to PATH"**

### 方式三：nvm-windows（推荐）

见 `nvm-版本管理.md`，安装后可随意切换版本。

验证：

```powershell
node --version
npm --version
```

> 如果命令找不到，重启终端或手动检查 PATH：`系统属性 → 环境变量 → Path`。

---

## 🐧 Linux

### Ubuntu / Debian（apt）

⚠️ apt 源的 Node.js 版本通常过旧，**推荐用 NodeSource PPA**：

```bash
# 安装 NodeSource PPA（以 22.x LTS 为例）
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -

# 安装
sudo apt install -y nodejs
```

如用 apt 直接装（版本可能老旧）：

```bash
sudo apt update
sudo apt install -y nodejs npm

# 查看版本（可能不是最新的）
node --version
```

### Fedora / RHEL（dnf）

```bash
# NodeSource PPA
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -

sudo dnf install -y nodejs
```

直接 dnf（可能版本较旧）：

```bash
sudo dnf install -y nodejs npm
```

### Arch Linux（pacman）

pacman 的 Node.js 版本通常较新：

```bash
sudo pacman -S nodejs npm

# 验证
node --version
npm --version
```

---

## ✅ 验证安装

```bash
node --version
# 输出示例: v22.x.x

npm --version
# 输出示例: 10.x.x
```

测试运行：

```bash
node -e "console.log('Hello Node.js');"
```

---

## 📁 安装路径差异

| 平台 | node 路径 | npm 全局包路径 |
|------|-----------|---------------|
| **macOS** (brew) | `/opt/homebrew/bin/node` | `/opt/homebrew/lib/node_modules` |
| **macOS** (pkg) | `/usr/local/bin/node` | `/usr/local/lib/node_modules` |
| **Windows** | `C:\Program Files\nodejs\` | `%APPDATA%\npm\node_modules` |
| **Linux** | `/usr/bin/node` | `/usr/lib/node_modules` |

---

## 🔗 参考

- 官网: https://nodejs.org
- NodeSource: https://github.com/nodesource/distributions
- 版本列表: https://nodejs.org/en/about/releases/
