# Oh My Zsh — Zsh 配置框架

> **平台**：macOS / Linux
> **Windows**：WSL 中可用

## 前置条件

```bash
# 确保 zsh 已安装
zsh --version   # 应 >= 5.0

# macOS 自带 zsh；Linux 可能需要安装
sudo apt install zsh         # Debian/Ubuntu
sudo dnf install zsh         # Fedora
sudo pacman -S zsh           # Arch
```

## 安装 Oh My Zsh

```bash
# 方式一：curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 方式二：wget
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**验证安装**：

```bash
echo $ZSH_THEME     # 输出当前主题（默认 robbyrussell）
echo $ZSH           # 输出 Oh My Zsh 安装路径（通常 ~/.oh-my-zsh）
cat ~/.zshrc        # 查看配置
```

## 核心文件：`~/.zshrc`

Oh My Zsh 的核心配置文件。主要参数：

```bash
# 主题
ZSH_THEME="robbyrussell"

# 插件（空格分隔）
plugins=(git docker npm)

# 自动更新（天）
zstyle ':omz:update' frequency 13

# 自动更正命令
ENABLE_CORRECTION="true"

# 启用命令补全大小写不敏感
CASE_SENSITIVE="false"
```

修改后生效：

```bash
source ~/.zshrc
```

## 主题

### 试用的方式

```bash
# 临时试用主题（不改文件）
ZSH_THEME="agnoster"
```

### 推荐主题

### powerlevel10k（最推荐）

```bash
# 1. 安装主题
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# 2. 设置 ~/.zshrc
ZSH_THEME="powerlevel10k/powerlevel10k"

# 3. 重启终端，按引导配置
```

特性：Git 状态显示、执行时间、命令提示、异步渲染。

### 其他主题

| 主题 | 说明 |
|------|------|
| `robbyrusell` | 默认，简洁 |
| `agnoster` | 经典三角形分隔，需要 Powerline 字体 |
| `avit` | 干净的上下结构 |
| `ys` | 显示完整路径+时间 |
| `half-life` | 极简 |

浏览所有主题：https://github.com/ohmyzsh/ohmyzsh/wiki/Themes

## 插件

### 安装插件

两种方式：

1. **内置插件**：直接加到 `plugins=()` 即可
2. **第三方插件**：克隆到 `$ZSH_CUSTOM/plugins/`

### 通用配置

```bash
# ~/.zshrc
plugins=(
    git
    zsh-autosuggestions
    zsh-syntax-highlighting
    web-search
    docker
    npm
    copyfile
    extract
    sudo
    colored-man-pages
    history
    alias-finder
)
```

### 必装第三方插件

```bash
# zsh-autosuggestions — 历史命令建议（灰色提示，按 → 补全）
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# zsh-syntax-highlighting — 命令语法高亮
git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### 实用内置插件速览

| 插件名 | 功能 |
|--------|------|
| `git` | Git 别名（gst=git status, ga=git add 等 100+） |
| `extract` | `x` 命令解压任何格式 |
| `web-search` | 终端直接搜索：`google xxx` `baidu xxx` `github xxx` |
| `copyfile` | `copyfile file.txt` 复制文件内容 |
| `copypath` | `copypath` 复制当前路径 |
| `sudo` | 按两下 Esc 在命令前加 sudo |
| `docker` | Docker 命令补全 |
| `npm` | npm 命令补全（`npm i` → `npm install`） |
| `history` | 历史搜索增强 |
| `alias-finder` | `alias-finder git status` 找匹配别名 |

## 别名

Oh My Zsh 自带 git 插件包含大量别名：

```bash
# 查看所有别名
alias

# 常用 git 别名
gst        = git status
ga         = git add
gcmsg      = git commit -m
gp         = git push
gl         = git pull
gco        = git checkout
gb         = git branch
gd         = git diff
glog       = git log --oneline --graph

# 自定义别名（在 ~/.zshrc 中添加）
alias ll='ls -la'
alias c='clear'
alias reload='source ~/.zshrc'
alias ip='curl ipinfo.io/ip'
```

## 更新

```bash
# 手动更新
omz update

# 自动更新（在 ~/.zshrc 中设置）
zstyle ':omz:update' mode auto      # 自动更新
zstyle ':omz:update' frequency 7    # 每 7 天
```

## 卸载

```bash
uninstall_oh_my_zsh
```

## 排查

```bash
# 检查 zsh 是否为默认 shell
echo $SHELL

# 切换默认 shell 到 zsh
chsh -s $(which zsh)

# 测试配置是否有语法错误
zsh -n ~/.zshrc
```

---

**参考**：https://ohmyz.sh/
