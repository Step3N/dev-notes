# 🐧 新 Linux 配置清单

> 从开箱到开工，逐项打勾 ✅
> 适用于 Ubuntu 24.04 LTS / Debian 系，其他发行版请替换包管理器命令

---

## 1. 系统更新

- [ ] **更新软件源和系统** — 新系统第一件事：确保所有包都是最新，避免潜在的兼容性问题
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
- [ ] **安装必要依赖** — 编译工具链和常用工具，很多软件的安装和编译依赖它们
  ```bash
  sudo apt install -y \
    build-essential \
    curl wget \
    git \
    zsh \
    unzip \
    software-properties-common \
    apt-transport-https \
    ca-certificates \
    gnupg \
    lsb-release
  ```
- [ ] **清理不必要的包** — 释放磁盘空间
  ```bash
  sudo apt autoremove -y && sudo apt autoclean
  ```
- [ ] **验证系统状态**
  ```bash
  lsb_release -a && uname -r
  ```

## 2. Shell 配置

- [ ] **设置 Zsh 为默认 Shell** — Zsh 比 Bash 有更好的补全、主题和插件生态
  ```bash
  chsh -s $(which zsh)
  # 退出重新登录，或执行:
  exec zsh
  ```
- [ ] **安装 Oh My Zsh** — Zsh 的配置管理框架
  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
- [ ] **安装 Powerlevel10k 主题** — 美观且信息丰富的提示符
  ```bash
  git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
  # 编辑 ~/.zshrc: ZSH_THEME="powerlevel10k/powerlevel10k"
  ```
- [ ] **安装 Zsh 插件** — 自动补全和语法高亮
  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  # 编辑 ~/.zshrc: plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
  source ~/.zshrc
  ```
- [ ] **配置别名** — 终端操作效率翻倍
  ```bash
  cat >> ~/.zshrc << 'EOF'
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
  EOF
  source ~/.zshrc
  ```
- [ ] **验证 Shell**
  ```bash
  echo $SHELL && echo $ZSH_VERSION && ll
  ```

## 3. 终端模拟器

- [ ] **安装 Kitty** (推荐) — GPU 加速的终端模拟器，速度快、配置灵活、支持字体连字
  ```bash
  curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
  # 创建符号链接
  ln -sf ~/.local/kitty.app/bin/kitty ~/.local/bin/
  # 设置为默认终端（可选）
  sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator ~/.local/bin/kitty 50
  ```
- [ ] **安装 GNOME Terminal** (Ubuntu 默认，轻量可选)
  ```bash
  sudo apt install -y gnome-terminal
  ```
- [ ] **安装 Nerd Font** — 终端图标符号支持
  ```bash
  # 手动下载 https://www.nerdfonts.com/font-downloads
  # 或一键安装 Meslo Nerd Font
  mkdir -p ~/.local/share/fonts
  cd /tmp
  wget https://github.com/ryanoasis/nerd-fonts/releases/latest/download/Meslo.zip
  unzip Meslo.zip -d ~/.local/share/fonts/
  fc-cache -fv
  ```
- [ ] **配置 Kitty 主题**
  ```bash
  # 编辑 ~/.config/kitty/kitty.conf
  mkdir -p ~/.config/kitty
  cat > ~/.config/kitty/kitty.conf << 'EOF'
  font_family      MesloLGS Nerd Font
  font_size        13
  enable_audio_bell no
  confirm_os_window_close 0
  shell_integration enabled
  EOF
  ```

## 4. Git 配置

- [ ] **配置用户名和邮箱**
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your.email@example.com"
  ```
- [ ] **配置 Git 关键选项**
  ```bash
  git config --global init.defaultBranch main
  git config --global core.editor "code --wait"
  git config --global core.autocrlf input
  git config --global pull.rebase true
  git config --global fetch.prune true
  ```
- [ ] **生成 SSH Key**
  ```bash
  ssh-keygen -t ed25519 -C "your.email@example.com"
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_ed25519
  ```
- [ ] **复制公钥**
  ```bash
  cat ~/.ssh/id_ed25519.pub
  # 复制输出，添加到 GitHub/GitLab → Settings → SSH Keys
  ```
- [ ] **验证 SSH**
  ```bash
  ssh -T git@github.com
  ```

## 5. 编程语言环境

- [ ] **安装 pyenv 依赖**
  ```bash
  sudo apt install -y make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
    libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
  ```
- [ ] **安装 pyenv** — Python 多版本管理
  ```bash
  curl https://pyenv.run | bash
  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
  echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
  echo 'eval "$(pyenv init -)"' >> ~/.zshrc
  source ~/.zshrc
  ```
- [ ] **安装 Python**
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
  node --version && npm --version
  ```
- [ ] **安装 pnpm**
  ```bash
  npm install -g pnpm
  ```
- [ ] **安装 Rust** — 系统编程语言，很多现代 CLI 工具用它编写
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  source "$HOME/.cargo/env"
  rustc --version && cargo --version
  ```
- [ ] **安装 Go** (可选)
  ```bash
  # 从 https://go.dev/dl/ 获取最新版下载链接
  wget https://go.dev/dl/go1.23.0.linux-amd64.tar.gz
  sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz
  echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc
  source ~/.zshrc
  go version
  ```

## 6. Docker

- [ ] **卸载旧版本 Docker**
  ```bash
  for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt remove -y $pkg; done
  ```
- [ ] **添加 Docker 官方源** — 获取最新的 Docker 版本
  ```bash
  # 添加 Docker 官方的 GPG 密钥
  sudo install -m 0755 -d /etc/apt/keyrings
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  sudo chmod a+r /etc/apt/keyrings/docker.gpg
  # 添加存储库
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt update
  ```
- [ ] **安装 Docker Engine**
  ```bash
  sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  ```
- [ ] **将当前用户加入 docker 组** — 避免每次敲 `sudo docker`
  ```bash
  sudo usermod -aG docker $USER
  newgrp docker  # 或退出重新登录
  ```
- [ ] **验证 Docker (无需 sudo)**
  ```bash
  docker --version && docker compose version
  docker run hello-world
  ```

## 7. VS Code

- [ ] **添加微软 GPG 密钥和源**
  ```bash
  wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
  sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
  sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
  rm -f packages.microsoft.gpg
  sudo apt update
  ```
- [ ] **安装 VS Code**
  ```bash
  sudo apt install -y code
  ```
- [ ] **登录同步设置**
  ```bash
  # VS Code → 设置 → 打开 Settings Sync → 登录 GitHub
  ```
- [ ] **验证**
  ```bash
  code --version
  ```

## 8. 输入法 (中文用户)

- [ ] **安装 fcitx5** — Linux 下最主流的中文输入法框架
  ```bash
  sudo apt install -y fcitx5 fcitx5-chinese-addons fcitx5-configtool
  ```
- [ ] **配置 fcitx5**
  ```bash
  # 设置输入法框架
  im-config -n fcitx5
  # 重启后生效
  ```
- [ ] **安装中文字体** — 防止中文显示为方块
  ```bash
  sudo apt install -y fonts-noto-cjk fonts-noto-color-emoji
  ```

## 9. 常用应用

- [ ] **安装基础工具**
  ```bash
  sudo apt install -y \
    htop btop \
    tmux \
    tree \
    jq \
    httpie \
    neofetch \
    net-tools \
    ripgrep \
    fd-find \
    bat
  # bat 命令在某些系统上为 batcat，创建别名
  echo 'alias bat="batcat"' >> ~/.zshrc
  ```
- [ ] **安装 Flathub 应用** (需要 flatpak)
  ```bash
  sudo apt install -y flatpak
  flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
  flatpak install -y flathub \
    org.obsidian.Obsidian \
    org.gimp.GIMP \
    org.inkscape.Inkscape \
    com.discordapp.Discord \
    org.telegram.desktop
  ```
- [ ] **安装 snap 应用** (Ubuntu)
  ```bash
  sudo snap install \
    obsidian \
    task \
    notion-snap-reborn
  ```

## 10. 网络与代理

- [ ] **配置代理** (如果需要)
  ```bash
  cat >> ~/.zshrc << 'EOF'
  # 代理配置
  alias proxy='export http_proxy=http://127.0.0.1:7890; export https_proxy=http://127.0.0.1:7890'
  alias unproxy='unset http_proxy https_proxy'
  EOF
  source ~/.zshrc
  ```

## 11. 验证清单

- [ ] **完整环境验证**
  ```bash
  echo "=== 开发环境验证 ==="
  echo "系统: $(lsb_release -ds)"
  echo "Shell: $SHELL ($(zsh --version | head -c15))"
  echo "Git: $(git --version)"
  echo "Python: $(python --version)"
  echo "Node: $(node --version)"
  echo "npm: $(npm --version)"
  echo "Rust: $(rustc --version)"
  echo "Docker: $(docker --version | head -c25)"
  echo "VS Code: $(code --version | head -1)"
  echo "=== 全部就绪 ✅ ==="
  ```

---

> ✅ 全部完成？恭喜，你的 Linux 已经准备好开工了！
