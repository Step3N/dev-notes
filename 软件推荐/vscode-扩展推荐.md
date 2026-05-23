# VS Code 推荐扩展

所有扩展支持 **macOS / Windows / Linux** 全平台。

---

## 语言支持

| 扩展名 | ID | 说明 |
|--------|----|------|
| Python | `ms-python.python` | Python 语言支持（补全、调试、Linter、Jupyter） |
| Pylance | `ms-python.vscode-pylance` | 高性能 Python 语言服务（搭配 Python 扩展使用） |
| ESLint | `dbaeumer.vscode-eslint` | JavaScript/TypeScript 代码静态检查 |
| Prettier | `esbenp.prettier-vscode` | 代码格式化（支持 JS/TS/JSON/CSS/Markdown 等） |
| Go | `golang.Go` | Go 语言支持（补全、调试、测试） |
| rust-analyzer | `rust-lang.rust-analyzer` | Rust 语言支持 |
| Error Lens | `usernamehw.errorlens` | 错误信息直接显示在代码行尾 |

---

## 效率提升

| 扩展名 | ID | 说明 |
|--------|----|------|
| GitLens | `eamodio.gitlens` | 增强 Git 功能：blame、历史、CodeLens |
| GitHub Copilot | `GitHub.copilot` | AI 代码补全 |
| GitHub Copilot Chat | `GitHub.copilot-chat` | Copilot 对话面板 |
| Live Share | `ms-vsliveshare.vsliveshare` | 实时协作编辑 |
| Path Intellisense | `christian-kohler.path-intellisense` | 文件路径自动补全 |
| Thunder Client | `rangav.vscode-thunder-client` | 轻量 API 调试（替代 Postman） |
| Todo Tree | `Gruntfuggly.todo-tree` | 高亮并管理 TODO/FIXME 注释 |

---

## 主题与 UI

| 扩展名 | ID | 说明 |
|--------|----|------|
| Material Icon Theme | `PKief.material-icon-theme` | 文件图标主题 |
| One Dark Pro | `zhuangtongfa.Material-theme` | One Dark 配色主题 |
| GitHub Theme | `GitHub.github-vscode-theme` | GitHub 官方主题（浅色/深色） |
| FiraCode Nerd Font | — | 需单独安装字体（支持连字） |

---

## Docker

| 扩展名 | ID | 说明 |
|--------|----|------|
| Docker | `ms-azuretools.vscode-docker` | Docker 容器镜像管理、Dockerfile 补全、docker-compose 支持 |

---

## 远程开发

| 扩展名 | ID | 说明 |
|--------|----|------|
| Remote - SSH | `ms-vscode-remote.remote-ssh` | 通过 SSH 连接远程服务器开发 |
| Remote - WSL | `ms-vscode-remote.remote-wsl` | **Windows 专用**，在 WSL 中开发 |
| Dev Containers | `ms-vscode-remote.remote-containers` | 在 Docker 容器中开发 |

---

## AI

| 扩展名 | ID | 说明 |
|--------|----|------|
| GitHub Copilot | `GitHub.copilot` | 行内 AI 代码补全 |
| GitHub Copilot Chat | `GitHub.copilot-chat` | AI 对话式编程助手 |
| Continue | `Continue.continue` | 开源 AI 编码助手（支持多种 LLM 后端） |

---

## 一键安装全部扩展

运行以下命令批量安装：

```bash
# macOS / Linux
code --install-extension ms-python.python \
  --install-extension ms-python.vscode-pylance \
  --install-extension dbaeumer.vscode-eslint \
  --install-extension esbenp.prettier-vscode \
  --install-extension golang.Go \
  --install-extension rust-lang.rust-analyzer \
  --install-extension usernamehw.errorlens \
  --install-extension eamodio.gitlens \
  --install-extension GitHub.copilot \
  --install-extension GitHub.copilot-chat \
  --install-extension ms-vsliveshare.vsliveshare \
  --install-extension christian-kohler.path-intellisense \
  --install-extension rangav.vscode-thunder-client \
  --install-extension Gruntfuggly.todo-tree \
  --install-extension PKief.material-icon-theme \
  --install-extension zhuangtongfa.Material-theme \
  --install-extension GitHub.github-vscode-theme \
  --install-extension ms-azuretools.vscode-docker \
  --install-extension ms-vscode-remote.remote-ssh \
  --install-extension ms-vscode-remote.remote-containers \
  --install-extension Continue.continue
```

```powershell
# Windows PowerShell
code --install-extension ms-python.python `
  --install-extension ms-python.vscode-pylance `
  --install-extension dbaeumer.vscode-eslint `
  --install-extension esbenp.prettier-vscode `
  --install-extension golang.Go `
  --install-extension rust-lang.rust-analyzer `
  --install-extension usernamehw.errorlens `
  --install-extension eamodio.gitlens `
  --install-extension GitHub.copilot `
  --install-extension GitHub.copilot-chat `
  --install-extension ms-vsliveshare.vsliveshare `
  --install-extension christian-kohler.path-intellisense `
  --install-extension rangav.vscode-thunder-client `
  --install-extension Gruntfuggly.todo-tree `
  --install-extension PKief.material-icon-theme `
  --install-extension zhuangtongfa.Material-theme `
  --install-extension GitHub.github-vscode-theme `
  --install-extension ms-azuretools.vscode-docker `
  --install-extension ms-vscode-remote.remote-ssh `
  --install-extension ms-vscode-remote.remote-containers `
  --install-extension Continue.continue
```

### 查看已安装扩展

```bash
code --list-extensions
```

### 导出扩展列表

```bash
code --list-extensions > extensions.txt
```

### 从文件批量安装

```bash
cat extensions.txt | xargs -L1 code --install-extension
```
