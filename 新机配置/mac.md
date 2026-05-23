# 🍎 新 Mac 配置清单

> 从开箱到开工，逐项打勾 ✅

---

## 1. 系统设置

- [ ] **显示隐藏文件** — Finder 中显示 `.` 开头的隐藏文件，方便查看 `.zshrc`、`.gitconfig` 等
  ```bash
  defaults write com.apple.finder AppleShowAllFiles -bool true && killall Finder
  ```
- [ ] **显示所有文件扩展名** — 避免同名不同格式文件混淆
  ```bash
  defaults write NSGlobalDomain AppleShowAllExtensions -bool true
  ```
- [ ] **设置键盘重复速度** — 提升长按按键时的重复输入速度（如 `hjkl` 导航）
  ```bash
  defaults write NSGlobalDomain KeyRepeat -int 1
  defaults write NSGlobalDomain InitialKeyRepeat -int 10
  ```
- [ ] **启用三指拖移** — 窗口拖拽更方便（辅助功能 → 指针控制 → 触控板选项）
  > ⚠️ macOS Sonoma+ 可能需手动去"系统设置 → 辅助功能 → 指针控制 → 触控板选项"中开启
  ```bash
  defaults write com.apple.AppleMultitouchTrackpad DragLock -bool false
  defaults write com.apple.AppleMultitouchTrackpad Dragging -bool true
  ```
- [ ] **关闭自动纠正/大写** — 编程时输入代码不被自动修正
  ```bash
  defaults write NSGlobalDomain NSAutomaticSpellingCorrectionEnabled -bool false
  defaults write NSGlobalDomain NSAutomaticCapitalizationEnabled -bool false
  defaults write NSGlobalDomain NSAutomaticPeriodSubstitutionEnabled -bool false
  ```
- [ ] **设置 Dock 自动隐藏** — 屏幕空间最大化
  ```bash
  defaults write com.apple.dock autohide -bool true && killall Dock
  ```
- [ ] **验证系统设置**
  ```bash
  # 检查隐藏文件是否显示
  defaults read com.apple.finder AppleShowAllFiles
  # 检查 KeyRepeat 值（应为 1）
  defaults read NSGlobalDomain KeyRepeat
  ```

## 2. Xcode CLI Tools

- [ ] **安装 Xcode Command Line Tools** — 提供 `git`、`cc`、`make` 等编译器核心工具链，Homebrew 也依赖它
  ```bash
  xcode-select --install
  ```
- [ ] **验证安装**
  ```bash
  xcode-select -p && gcc --version && git --version
  ```

## 3. 包管理器 — Homebrew

- [ ] **安装 Homebrew** — macOS 的包管理器，安装 CLI 和 GUI 应用的统一入口
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
- [ ] **添加到 PATH**（Apple Silicon Mac 需要手动添加）
  ```bash
  echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
  eval "$(/opt/homebrew/bin/brew shellenv)"
  ```
- [ ] **验证安装**
  ```bash
  brew doctor && brew --version
  ```

## 4. 终端与 Shell

- [ ] **安装 iTerm2** — 比原生终端功能更强（分屏、搜索、热键窗口）
  ```bash
  brew install --cask iterm2
  ```
- [ ] **安装 Oh My Zsh** — Zsh 配置管理框架，主题/插件一键管理
  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
- [ ] **安装推荐主题**（例如 Powerlevel10k） — 信息丰富的美观提示符
  ```bash
  git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
  # 然后在 ~/.zshrc 中设置 ZSH_THEME="powerlevel10k/powerlevel10k"
  ```
- [ ] **安装常用 Zsh 插件** — 自动补全、语法高亮等提升终端体验
  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  # 然后在 ~/.zshrc 的 plugins 中添加 zsh-autosuggestions 和 zsh-syntax-highlighting
  ```
- [ ] **安装现代 CLI 替代工具** (可选) — 更友好的替代命令
  ```bash
  brew install bat ripgrep fd fzf jq tree htop
  ```

## 5. Shell 配置 (.zshrc)

- [ ] **配置 .zshrc 别名** — 快捷操作，提升日常效率
  ```bash
  cat >> ~/.zshrc << 'EOF'
  # 常用别名
  alias ll='ls -lah'
  alias la='ls -A'
  alias l='ls -F'
  alias ..='cd ..'
  alias ...='cd ../..'
  alias c='clear'
  alias g='git'
  alias gc='git clone'
  alias gs='git status'
  alias gp='git push'
  alias v='nvim'
  alias vim='nvim'

  # 如果代理在运行
  alias proxy='export http_proxy=http://127.0.0.1:7890; export https_proxy=http://127.0.0.1:7890'
  alias unproxy='unset http_proxy https_proxy'
  EOF
  source ~/.zshrc
  ```
- [ ] **验证配置**
  ```bash
  echo $SHELL && echo $ZSH_VERSION
  # 检查别名是否生效
  alias ll
  ```

## 6. Git

- [ ] **配置用户名和邮箱** — 每次 commit 的身份信息
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```
- [ ] **配置 Git 默认分支和编辑器**
  ```bash
  git config --global init.defaultBranch main
  git config --global core.editor "code --wait"
  ```
- [ ] **生成 SSH Key** — 免密推送 GitHub/GitLab
  ```bash
  ssh-keygen -t ed25519 -C "your.email@example.com"
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  ```
- [ ] **复制公钥到剪贴板** — 然后粘贴到 GitHub Settings → SSH and GPG keys
  ```bash
  pbcopy < ~/.ssh/id_ed25519.pub
  echo "已复制到剪贴板，请添加到 GitHub"
  ```
- [ ] **验证 SSH 连接**
  ```bash
  ssh -T git@github.com
  # 应看到 "Hi username! You've successfully authenticated..."
  ```

## 7. 编程语言环境

- [ ] **安装 pyenv** — Python 多版本管理，系统自带 Python 不要直接用
  ```bash
  brew install pyenv
  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
  echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
  echo 'eval "$(pyenv init -)"' >> ~/.zshrc
  source ~/.zshrc
  ```
- [ ] **安装 Python （最新稳定版）**
  ```bash
  pyenv install 3.12
  pyenv global 3.12
  python --version
  ```
- [ ] **安装 nvm** — Node.js 多版本管理
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  source ~/.zshrc
  ```
- [ ] **安装 Node.js LTS**
  ```bash
  nvm install --lts
  nvm use --lts
  node --version && npm --version
  ```
- [ ] **安装 pnpm** — 更快的 Node 包管理器
  ```bash
  npm install -g pnpm
  ```

## 8. Docker

- [ ] **安装 Docker Desktop** — 容器化开发环境必备
  ```bash
  brew install --cask docker
  ```
- [ ] **启动 Docker 并验证**
  ```bash
  open -a Docker
  # 等待启动后
  docker --version && docker compose version
  ```

## 9. VS Code

- [ ] **安装 VS Code**
  ```bash
  brew install --cask visual-studio-code
  ```
- [ ] **安装 `code` 命令** — 在终端能直接 `code .` 打开项目
  ```bash
  # 在 VS Code 中按 Cmd+Shift+P → 输入 "Shell Command: Install 'code' command in PATH"
  # 或手动添加:
  echo 'export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"' >> ~/.zshrc
  ```
- [ ] **登录 GitHub 同步设置** — 同步扩展、配置、快捷键（使用 Settings Sync）
  ```bash
  # VS Code → 设置 → 打开 Settings Sync → 登录 GitHub 并启用
  ```
- [ ] **验证**
  ```bash
  code --version && code --list-extensions
  ```

## 10. 浏览器

- [ ] **安装 Chrome**
  ```bash
  brew install --cask google-chrome
  ```
- [ ] **安装 Firefox** (可选)
  ```bash
  brew install --cask firefox
  ```
- [ ] **登录浏览器同步** — 同步书签、密码、扩展

## 11. 常用应用 (Brew Bundle)

- [ ] **安装日常应用合集** — 一行搞定所有常用软件
  ```bash
  brew install --cask \
    arc \
    raycast \
    spotify \
    iina \
    obsidian \
    1password \
    notion \
    figma \
    telegram \
    discord
  ```
- [ ] **安装命令行工具**
  ```bash
  brew install \
    curl wget \
    htop btop \
    tmux \
    tree \
    jq yq \
    httpie \
    lazygit \
    neofetch
  ```

## 12. 备份与恢复

- [ ] **恢复 dotfiles** — 将之前备份的 `.zshrc`、`.gitconfig`、`Brewfile` 等复制回来
  ```bash
  # 如果 dotfiles 在 GitHub 上
  git clone git@github.com:yourname/dotfiles.git ~/dotfiles
  # 执行 restore 脚本，或手动复制
  ```
- [ ] **恢复 SSH 密钥** — 从备份或密码管理器恢复
  ```bash
  # 从 1Password 等导出，或复制备份的 ~/.ssh 目录
  ```
- [ ] **恢复 GPG 密钥** (如有)
  ```bash
  gpg --import ~/backup/gpg-private.key
  ```
- [ ] **验证完整环境**
  ```bash
  # 运行一个简单的 "Hello World" 确认一切正常
  python -c "print('macOS dev env ready ✅')"
  node -e "console.log('Node.js ready ✅')"
  git --version && docker --version && code --version
  ```

---

> ✅ 全部完成？恭喜，你的 Mac 已经准备好开工了！
