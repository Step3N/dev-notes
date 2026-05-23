# 现代 CLI 工具箱

> **平台**：全平台（安装方式因系统而异）

经典 Unix 工具的现代替代品，更快、更易用、更美观。

## 核心替换

| 旧工具 | 新工具 | 说明 | 安装 | 示例 |
|--------|--------|------|------|------|
| `cat` | **bat** | 语法高亮文件，集成 git 改动标记 | `brew install bat` / `apt install bat` / `cargo install bat` | `bat file.rs` / `bat --show-all file` |
| `find` | **fd** | 直觉式语法，默认忽略隐藏/`.gitignore` | `brew install fd` / `apt install fd-find` / `cargo install fd-find` | `fd pattern` / `fd -e md` |
| `grep` | **ripgrep (rg)** | 递归搜索，自动尊重 `.gitignore` | `brew install ripgrep` / `apt install ripgrep` / `cargo install ripgrep` | `rg "TODO"` / `rg -l "function" src/` |
| `ls` | **eza** | 彩色输出，树形视图，图标支持 | `brew install eza` / `apt install eza` / `cargo install eza` | `eza -la` / `eza --tree` |
| `less` `tail` | **delta** | Git diff 高亮查看器 | `brew install git-delta` / `cargo install git-delta` | 配合 git：`[core] pager = delta` |
| `cut` `awk` | **jq** | JSON 命令行处理器 | `brew install jq` / `apt install jq` / `winget install jq` | `curl api | jq '.data'` / `jq -r '.name' file.json` |
| `sed` `awk` | **sd** | 字符串替换，直觉式语法 | `cargo install sd` | `sd "old" "new" file.txt` / `sd "foo(\d+)" "bar$1"` |

```bash
# bat — 查看带行号和语法高亮的文件
bat ~/.zshrc

# fd — 快速查找 Markdown 文件
fd -e md

# rg — 在项目中搜索所有 TODO
rg "TODO" --type-add 'web:*.{html,css,js}' -t web

# eza — 树形目录结构
eza --tree -L 3

# jq — 从 JSON 提取字段
curl -s https://jsonplaceholder.typicode.com/posts/1 | jq '{title: .title, id: .id}'

# sd — 批量替换文本
sd "example.com" "new-example.com" config/*
```

## 搜索与导航

| 工具 | 说明 | 安装 | 示例 |
|------|------|------|------|
| **fzf** | 通用模糊查找器，`Ctrl+R` 搜索历史神器 | `brew install fzf` / `apt install fzf` | `Ctrl+R` 搜索命令 / `**<Tab>` 补全路径 |
| **zoxide** | 智能 `cd`，自动学习你的目录习惯 | `brew install zoxide` / `apt install zoxide` | `z pro` 跳到最近访问的 `project/` |

```bash
# 安装 fzf 并启用快捷键
brew install fzf
$(brew --prefix)/opt/fzf/install

# 之后在终端中按 Ctrl+R 体验增强版历史搜索

# zoxide — 跳过 cd 直接跳转
zoxide init zsh
z Documents     # 跳到 ~/Documents
z pro           # 跳到最近访问的 project 目录
```

## 信息查看

| 工具 | 说明 | 安装 | 示例 |
|------|------|------|------|
| **tldr** | 简化版 man 手册，带示例 | `brew install tldr` / `apt install tldr` | `tldr curl` / `tldr tar` |
| **duf** | 磁盘使用情况概览 | `brew install duf` / `apt install duf` | `duf` |
| **procs** | 现代化 `ps` | `brew install procs` / `cargo install procs` | `procs` / `procs nginx` |
| **bottom (btm)** | 现代化资源监视器 | `brew install bottom` / `cargo install bottom` | `btm` |

```bash
# tldr — 快速查看命令用法
tldr curl

# duf — 各挂载点磁盘用量
duf

# procs — 按 CPU 排序查看进程
procs --sort cpu

# btm — 实时资源监控
btm
```

## 网络与 API

| 工具 | 说明 | 安装 | 示例 |
|------|------|------|------|
| **httpie** | 现代化 curl，输出友好 | `brew install httpie` / `apt install httpie` / `pip install httpie` | `http GET https://api.example.com` |
| **dog / doggo** | 现代化 DNS 查询 | `brew install dog` / `cargo install dog` | `dog example.com MX` |

```bash
# httpie — 比 curl 更直观
http https://jsonplaceholder.typicode.com/posts/1
http POST https://jsonplaceholder.typicode.com/posts title=hello body=world

# dog — DNS 查询
dog example.com A
dog example.com MX @1.1.1.1
```

## 对比演示

```bash
# find vs fd: 找所有 .py 文件
find . -name "*.py"              # 慢，语法繁琐
fd -e py                         # 快，简单

# grep vs rg: 递归搜索
grep -r "error" --include="*.log" .  # 需要手动指定模式
rg "error" -g "*.log"                # 自动 .gitignore 感知

# ls vs eza
ls -la
eza -la --icons                    # 带图标，颜色更丰富
```

## 快捷配置（zsh）

在 `~/.zshrc` 中添加别名：

```bash
alias ls="eza --icons"
alias ll="eza -la --icons"
alias lt="eza --tree --icons"
alias cat="bat"
alias grep="rg"
alias find="fd"
alias ps="procs"
alias top="btm"
alias df="duf"
alias man="tldr"
alias cd="z"
```

## 验证安装

```bash
# 检查各工具版本
bat --version
fd --version
rg --version
eza --version
jq --version
fzf --version
tldr --version
```

---

**参考**：
- bat: https://github.com/sharkdp/bat
- fd: https://github.com/sharkdp/fd
- ripgrep: https://github.com/BurntSushi/ripgrep
- eza: https://github.com/eza-community/eza
- delta: https://github.com/dandavison/delta
- jq: https://jqlang.github.io/jq/
- fzf: https://github.com/junegunn/fzf
- zoxide: https://github.com/ajeetdsouza/zoxide
- tldr: https://github.com/tldr-pages/tldr
