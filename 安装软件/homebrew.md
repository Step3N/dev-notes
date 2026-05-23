# Homebrew — macOS 包管理器

macOS 上最流行的第三方包管理器，也叫 **brew**。它能装 CLI 工具、桌面应用、字体、驱动等等。

---

## 📦 安装

一行命令搞定：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

装完验证：

```bash
brew --version
# 输出示例: Homebrew 4.x.x
```

> **Intel Mac** 默认装在 `/usr/local`，**Apple Silicon** 装在 `/opt/homebrew`。
> 装完后留意终端提示，可能需要把 brew 加到 PATH，一般是：
> ```bash
> eval "$(/opt/homebrew/bin/brew shellenv)"
> ```

---

## ⚙️ 常用命令

| 操作 | 命令 |
|------|------|
| 安装 formula | `brew install <包名>` |
| 安装 cask | `brew install --cask <应用名>` |
| 卸载 | `brew uninstall <包名>` |
| 搜索 | `brew search <关键词>` |
| 查看信息 | `brew info <包名>` |
| 列出已装 | `brew list` |
| 更新 brew 自身 | `brew update` |
| 升级所有包 | `brew upgrade` |
| 清理旧版本 | `brew cleanup` |
| 检查系统问题 | `brew doctor` |

---

## 🔧 Formulas vs Casks

- **Formula** — 命令行工具，如 `git`、`wget`、`node`。装在 `/opt/homebrew/bin/`。
- **Cask** — GUI 桌面应用，如 `google-chrome`、`visual-studio-code`、`docker`。装在 `/Applications/`。

查看已装的 cask：

```bash
brew list --cask
```

---

## 💡 Brew Bundle

用 `Brewfile` 统一管理所有包，适合备份和新机恢复。

生成当前环境的 Brewfile：

```bash
brew bundle dump --force --file=~/Brewfile
```

从 Brewfile 恢复：

```bash
brew bundle --file=~/Brewfile
```

Brewfile 示例：

```ruby
tap "homebrew/cask"
tap "homebrew/cask-fonts"

brew "git"
brew "node"
brew "wget"

cask "google-chrome"
cask "visual-studio-code"
cask "iterm2"
cask "font-fira-code"

mas "Xcode", id: 497799835
```

> `mas` 是 Mac App Store 命令行工具，可配合 brew 一起用：`brew install mas`

---

## ⚠️ 常见问题

### 权限错误

```bash
# 修复 /usr/local 或 /opt/homebrew 目录权限
sudo chown -R $(whoami) $(brew --prefix)/*
```

### Tap（第三方程源）

```bash
# 添加 tap
brew tap homebrew/cask-fonts

# 查看所有 tap
brew tap

# 安装 tap 里的包
brew install font-fira-code
```

### 清理磁盘空间

```bash
# 删除所有旧版本（降级用 brew pin）
brew cleanup --prune=all

# 查看能清理多少
brew cleanup --dry-run
```

### Homebrew 很慢

```bash
# 使用国内镜像（中科大）
git -C "$(brew --repo)" remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 恢复官方源
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git
```

---

## 🔗 参考

- 官网: https://brew.sh
- 源码: https://github.com/Homebrew/brew
