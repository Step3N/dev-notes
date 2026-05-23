# Git 初始配置

新装 Git 后的第一件事。

---

## 1. 设置用户名和邮箱

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

用你 GitHub / GitLab 注册的邮箱，commit 记录会关联到账号。

---

## 2. 配置默认分支名

```bash
git config --global init.defaultBranch main
```

以后 `git init` 创建的分支默认叫 `main`，而不是 `master`。

---

## 3. 配置默认编辑器

```bash
# VS Code（推荐）
git config --global core.editor "code --wait"

# Vim
git config --global core.editor "vim"

# Neovim
git config --global core.editor "nvim"
```

---

## 4. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

按提示操作，建议直接回车用默认路径 `~/.ssh/id_ed25519`，也可以设个 passphrase。

如果系统不支持 ed25519（极少见），改用 RSA：

```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```

---

## 5. 启动 ssh-agent 并添加密钥

**macOS**（推荐用系统自带的方式，自动管理）：

```bash
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

如果不想用钥匙串：

```bash
ssh-add ~/.ssh/id_ed25519
```

**Linux**：

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**Windows**（PowerShell 管理员模式）：

```powershell
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

### 开机自启 ssh-agent

**macOS / Linux** 在 `~/.ssh/config` 加：

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
```

**Windows** 将 ssh-agent 设为自动启动：

```powershell
Set-Service ssh-agent -StartupType Automatic
```

---

## 6. 添加公钥到远程平台

复制公钥：

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub
# 手动复制输出

# Windows
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard
```

然后粘贴到对应平台：

- **GitHub**: Settings -> SSH and GPG keys -> New SSH key
- **GitLab**: Preferences -> SSH Keys -> Add key
- **Gitee**: 设置 -> SSH 公钥 -> 添加公钥

---

## 7. 测试连接

```bash
ssh -T git@github.com
# Hi username! You've successfully authenticated...
```

如果第一次连，会提示确认 host key，输入 `yes` 即可。

换成 GitLab 就测：

```bash
ssh -T git@gitlab.com
```

---

## 8. 全局 .gitignore

创建全局忽略规则，避免每个项目都写 `.DS_Store`、`Thumbs.db`：

```bash
git config --global core.excludesFile ~/.gitignore_global
```

写入常用规则：

```bash
cat >> ~/.gitignore_global << 'EOF'
# macOS
.DS_Store
.DS_Store?
.AppleDouble
.LSOverride

# Windows
Thumbs.db
ehthumbs.db
desktop.ini

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS files
.Directory
EOF
```

---

## 9. 验证全部配置

```bash
git config --list --global
```

会输出所有已设置项，确认 `user.name`、`user.email`、`core.editor`、`init.defaultBranch` 都正确。
