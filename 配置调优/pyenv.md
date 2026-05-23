# pyenv — Python 版本管理

pyenv 让你在同一台机器上安装和切换多个 Python 版本，无需系统全局安装冲突。

---

## 安装

### macOS

```bash
brew install pyenv
```

安装后把以下内容加到 shell 配置文件（`~/.zshrc` 或 `~/.bashrc`）：

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

重新加载配置：

```bash
source ~/.zshrc
```

### Windows

用 **pyenv-win**（非官方 fork，但最稳定）：

```powershell
# 方式一：git clone
git clone https://github.com/pyenv-win/pyenv-win.git "$HOME\.pyenv"

# 方式二：pip
pip install pyenv-win --target "%USERPROFILE%\.pyenv"
```

手动添加环境变量（或重启终端）：

```powershell
[System.Environment]::SetEnvironmentVariable('PYENV', "$env:USERPROFILE\.pyenv", 'User')
[System.Environment]::SetEnvironmentVariable('PYENV_ROOT', "$env:USERPROFILE\.pyenv", 'User')
[System.Environment]::SetEnvironmentVariable('PYENV_HOME', "$env:USERPROFILE\.pyenv", 'User')
```

把 `%USERPROFILE%\.pyenv\bin` 和 `%USERPROFILE%\.pyenv\shims` 加到 PATH。

### Linux

```bash
# 方式一：自动安装（推荐）
curl https://pyenv.run | bash

# 方式二：手动 git clone
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

同样需要配置 shell：

```bash
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

---

## 安装编译依赖（Linux 关键！）

**Linux 上 pyenv 是从源码编译 Python，必须安装构建依赖**，否则报错或缺少功能：

```bash
# Ubuntu / Debian
sudo apt update
sudo apt install build-essential libssl-dev zlib1g-dev \
  libbz2-dev libreadline-dev libsqlite3-dev curl \
  libncursesw5-dev xz-utils tk-dev libxml2-dev \
  libxmlsec1-dev libffi-dev liblzma-dev -y

# Fedora
sudo dnf install make gcc zlib-devel bzip2 bzip2-devel \
  readline-devel sqlite sqlite-devel openssl-devel \
  tk-devel libffi-devel xz-devel -y
```

---

## 常用命令

```bash
# 列出所有可安装版本
pyenv install --list

# 安装特定版本
pyenv install 3.12.3
pyenv install 3.11.9
pyenv install 3.10.14

# 查看已安装版本
pyenv versions

# 查看当前使用版本
pyenv version

# 全局默认版本（影响整个系统）
pyenv global 3.12.3

# 当前目录及子目录使用特定版本
pyenv local 3.11.9

# 当前 shell session 临时切换
pyenv shell 3.10.14
```

验证：

```bash
python --version   # 应该显示 pyenv 设置的版本
pyenv which python # 查看实际路径
```

---

## 常见问题

### pyenv: command not found

- 检查 PATH 配置是否已添加到 shell 配置文件
- 重启终端或重新 `source ~/.zshrc`
- **macOS/Linux**：确认 `$PYENV_ROOT/bin` 在 PATH 中
- **Windows**：检查系统环境变量是否设置正确

### 编译错误 / BUILD FAILED

- **Linux**：确认所有构建依赖已安装（见上方命令）
- **macOS**：运行 `xcode-select --install`，可能需要重启
- 查看详细错误：`pyenv install 3.12.3 -v`

### 切换版本后 pip 还是旧版

```bash
# 确保 pip 也切换到对应版本
pip install --upgrade pip
```

---

## 卸载

```bash
# 卸载特定版本
pyenv uninstall 3.10.14

# 完全卸载 pyenv
# macOS: brew uninstall pyenv
# Linux/Win: rm -rf ~/.pyenv
# 并删除 shell 配置中的 pyenv 相关行
```
