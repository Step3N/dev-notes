# dev-notes

> **换电脑 → 搭环境 → 写代码。一个仓库就够了。**

每次换系统都要重搜一遍"怎么装 Python""怎么配 CUDA""怎么跑 Docker"？

**dev-notes 把完整搭建链路整理好了，从零到一，一条龙走到底。**

| 🧠 Python 深度学习 | 🤖 AI 编程助手 | 🌐 前后端项目 | 🎨 终端美化 |
|:---:|:---:|:---:|:---:|
| Python→CUDA→PyTorch→GPU 验证 | API Key→IDE→本地模型 | Node→DB→Vite→Express→部署 | 字体→Shell→Prompt→插件 |

**5 条完整链路 · 103 篇参考笔记 · macOS / Windows / Linux 全平台**

每条命令可复制可验证，不再东拼西凑。

[![GitHub stars](https://img.shields.io/github/stars/Step3N/dev-notes?style=for-the-badge&logo=github)](https://github.com/Step3N/dev-notes/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/Step3N/dev-notes?style=for-the-badge&logo=github)](https://github.com/Step3N/dev-notes/network)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Windows%20%7C%20Linux-blue?style=for-the-badge&logo=windows-terminal)](./)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)](./LICENSE)

---

## 🆕 第一次配环境？从这里开始

如果你是**第一次配置开发环境**，不知道从哪下手，跟着下面这条链路走：

| 推荐 | 链路 | 难度 | 做什么 |
|------|------|------|--------|
| ⭐ | [Python 深度学习环境搭建](./学习教程/python-深度学习环境搭建.md) | ★☆☆ | 从装 Python 到 GPU 跑通深度学习，最通用的起点 |
| ⭐ | [AI 编程环境搭建](./学习教程/ai-编程环境搭建.md) | ★☆☆ | 配好 AI 编程助手，以后开发效率翻倍 |
| ⭐ | [终端美化增强](./学习教程/终端美化增强.md) | ★☆☆ | 让终端好看又好用，每天看着心情好 |

> 💡 **建议顺序**：先配 Python 环境 → 再配 AI 编程助手 → 最后美化终端。每步都要验证成功再下一步。

---

## 🚀 一条龙搭建方案

从零开始，跟着走完每一步。

### 如果你是 All Developers

| # | 链路 | 覆盖内容 | 耗时 |
|---|------|---------|------|
| 🎨 | [终端美化增强](./学习教程/终端美化增强.md) | Nerd Font → 终端模拟器 → Shell → Prompt → 插件 → 颜色方案 | ~25min |

### 如果你是 AI / 数据科学开发者

| # | 链路 | 覆盖内容 | 耗时 |
|---|------|---------|------|
| 🧠 | [Python 深度学习环境搭建](./学习教程/python-深度学习环境搭建.md) | Python → pip 源 → Miniconda → CUDA → PyTorch → GPU 验证 | ~40min |
| 🤖 | [AI 编程环境搭建](./学习教程/ai-编程环境搭建.md) | API Key → Continue/Cursor → Ollama → 方案对比 | ~30min |

### 如果你是 Web / 全栈开发者

| # | 链路 | 覆盖内容 | 耗时 |
|---|------|---------|------|
| 🌐 | [Node.js 前后端项目搭建](./学习教程/nodejs-前后端搭建.md) | nvm → Vite+React → Express → PostgreSQL → Docker 部署 | ~50min |
| 🐳 | [Docker 项目运行](./学习教程/docker-项目运行.md) | 安装 → Dockerfile → Compose → Nginx → 开发/生产环境 | ~40min |

---

## 📌 快速选择你的系统

| 🍎 **macOS** | 🪟 **Windows** | 🐧 **Linux** |
|--------------|----------------|---------------|
| [新机配置清单](./新机配置/mac.md) | [新机配置清单](./新机配置/windows.md) | [新机配置清单](./新机配置/linux.md) |
| [Homebrew](./安装软件/homebrew.md) | [Winget](./安装软件/winget.md) | [包管理器对比](./配置调优/包管理器对比.md) |
| [iTerm2 配置](./配置调优/iterm2.md) | [Windows Terminal](./配置调优/windows-terminal.md) | [终端美化](./配置调优/linux-终端美化.md) |
| [Oh My Zsh](./配置调优/oh-my-zsh.md) | [Oh My Posh](./配置调优/oh-my-posh.md) | [Starship](./配置调优/starship.md) |
| [macOS 系统设置](./配置调优/macos-系统设置.md) | [WSL2 配置](./配置调优/wsl2.md) | [Linux 环境优化](./配置调优/linux-环境优化.md) |

---

## 📖 参考手册

按场景查找，快速定位到具体问题。

### 新机配置
| 文件 | 说明 |
|------|------|
| [mac.md](./新机配置/mac.md) | macOS 从头配置清单 |
| [windows.md](./新机配置/windows.md) | Windows 从头配置清单 |
| [linux.md](./新机配置/linux.md) | Linux 从头配置清单 |

### 安装软件
| 文件 | 说明 |
|------|------|
| [homebrew.md](./安装软件/homebrew.md) | macOS 包管理器 |
| [winget.md](./安装软件/winget.md) | Windows 包管理器 |
| [git.md](./安装软件/git.md) | Git 三平台安装 |
| [python.md](./安装软件/python.md) | Python 三平台安装 |
| [nodejs.md](./安装软件/nodejs.md) | Node.js 三平台安装 |
| [miniconda.md](./安装软件/miniconda.md) | Miniconda 安装与基础使用 |
| [pytorch.md](./安装软件/pytorch.md) | PyTorch CPU/GPU 安装 |
| [go-rust-java.md](./安装软件/go-rust-java.md) | Go/Rust/Java 环境简记 |
| [docker.md](./安装软件/docker.md) | Docker 三平台安装 |
| [database.md](./安装软件/database.md) | MySQL / PostgreSQL / Redis 安装 |
| [vscode.md](./安装软件/vscode.md) | VS Code 安装 |
| [neovim.md](./安装软件/neovim.md) | Neovim 编辑器 |
| [jetbrains.md](./安装软件/jetbrains.md) | JetBrains 全家桶 |
| [sublime-text.md](./安装软件/sublime-text.md) | Sublime Text 编辑器 |
| [ollama.md](./安装软件/ollama.md) | Ollama 本地模型运行 |
| [lm-studio.md](./安装软件/lm-studio.md) | LM Studio 图形化运行 |
| [llama-cpp.md](./安装软件/llama-cpp.md) | llama.cpp 推理引擎 |
| [jupyter.md](./安装软件/jupyter.md) | Jupyter Notebook/Lab 安装配置 |

### 配置调优
| 文件 | 说明 |
|------|------|
| [git-初始配置.md](./配置调优/git-初始配置.md) | Git 用户名/邮箱/SSH 配置 |
| [git-commit规范.md](./配置调优/git-commit规范.md) | Conventional Commits |
| [gitignore.md](./配置调优/gitignore.md) | .gitignore 编写指南 |
| [git-代理.md](./配置调优/git-代理.md) | Git 走代理 |
| [pip-换源.md](./配置调优/pip-换源.md) | pip 镜像源配置 |
| [pip-代理.md](./配置调优/pip-代理.md) | pip 走代理 |
| [npm-换源.md](./配置调优/npm-换源.md) | npm 镜像源配置 |
| [npm-代理.md](./配置调优/npm-代理.md) | npm 走代理 |
| [docker-代理.md](./配置调优/docker-代理.md) | Docker 走代理 |
| [nvm.md](./配置调优/nvm.md) | nvm 版本管理 |
| [pyenv.md](./配置调优/pyenv.md) | pyenv 版本管理 |
| [cuda-配置.md](./配置调优/cuda-配置.md) | CUDA + cuDNN 安装配置 |
| [ssh.md](./配置调优/ssh.md) | SSH 配置管理 |
| [wsl2.md](./配置调优/wsl2.md) | WSL2 从零到一 |
| [oh-my-zsh.md](./配置调优/oh-my-zsh.md) | Oh My Zsh 配置 |
| [oh-my-posh.md](./配置调优/oh-my-posh.md) | Oh My Posh 配置 |
| [starship.md](./配置调优/starship.md) | Starship 跨平台 Prompt |
| [tmux.md](./配置调优/tmux.md) | tmux 终端复用器 |
| [iterm2.md](./配置调优/iterm2.md) | iTerm2 终端配置 |
| [windows-terminal.md](./配置调优/windows-terminal.md) | Windows Terminal 配置 |
| [linux-终端美化.md](./配置调优/linux-终端美化.md) | Linux 终端美化 |
| [环境变量管理.md](./配置调优/环境变量管理.md) | direnv / dotenv / PATH |
| [macos-系统设置.md](./配置调优/macos-系统设置.md) | macOS 开发优化 |
| [windows-开发模式.md](./配置调优/windows-开发模式.md) | Windows 开发者模式 |
| [linux-环境优化.md](./配置调优/linux-环境优化.md) | Linux 开发调优 |
| [vscode-配置同步.md](./配置调优/vscode-配置同步.md) | VS Code Settings Sync |
| [包管理器对比.md](./配置调优/包管理器对比.md) | apt/dnf/pacman 命令对比 |
| [包管理器总结.md](./配置调优/包管理器总结.md) | 全平台包管理器横向对比 |

### 日常使用
| 文件 | 说明 |
|------|------|
| [git-日常命令.md](./日常使用/git-日常命令.md) | add/commit/push/pull |
| [git-分支管理.md](./日常使用/git-分支管理.md) | branch/merge/rebase |
| [git-撤销回退.md](./日常使用/git-撤销回退.md) | reset/revert/restore |
| [git-高级技巧.md](./日常使用/git-高级技巧.md) | stash/reflog/hooks |
| [gh.md](./日常使用/gh.md) | GitHub CLI 命令 |
| [docker-常用命令.md](./日常使用/docker-常用命令.md) | images/ps/exec/logs |
| [docker-compose.md](./日常使用/docker-compose.md) | 多容器编排 |
| [dockerfile.md](./日常使用/dockerfile.md) | Dockerfile 语法 |
| [podman.md](./日常使用/podman.md) | Podman 替代方案 |
| [vscode-快捷键.md](./日常使用/vscode-快捷键.md) | VS Code 常用快捷键 |
| [vscode-调试.md](./日常使用/vscode-调试.md) | launch.json 调试配置 |
| [终端快捷键.md](./日常使用/终端快捷键.md) | 终端操作效率 |
| [现代CLI工具.md](./日常使用/现代CLI工具.md) | ripgrep/fd/fzf/bat/jq |
| [cron.md](./日常使用/cron.md) | 定时任务配置 |

### 学习教程
| 文件 | 说明 |
|------|------|
| [终端美化增强.md](./学习教程/终端美化增强.md) | 🎨 三平台终端美化从字体到插件 |
| [python-深度学习环境搭建.md](./学习教程/python-深度学习环境搭建.md) | 🧠 深度学习环境从零到一 |
| [ai-编程环境搭建.md](./学习教程/ai-编程环境搭建.md) | 🤖 AI 编程助手配置全流程 |
| [nodejs-前后端搭建.md](./学习教程/nodejs-前后端搭建.md) | 🌐 全栈项目搭建 |
| [docker-项目运行.md](./学习教程/docker-项目运行.md) | 🐳 Docker 项目运行 |
| [github-actions.md](./学习教程/github-actions.md) | CI/CD 从零到一 |
| [nginx.md](./学习教程/nginx.md) | Nginx 反向代理/HTTPS |
| [yarn-pnpm.md](./学习教程/yarn-pnpm.md) | Yarn / pnpm 包管理器 |
| [python-虚拟环境.md](./学习教程/python-虚拟环境.md) | venv / poetry / conda |
| [pip-包清单.md](./学习教程/pip-包清单.md) | 常用 pip 包推荐 |
| [npm-全局包.md](./学习教程/npm-全局包.md) | 常用 npm 全局包 |
| [aider.md](./学习教程/aider.md) | 终端 AI 结对编程 |
| [continue.md](./学习教程/continue.md) | VS Code AI 扩展 |
| [本地模型推荐.md](./学习教程/本地模型推荐.md) | 本地运行模型选型 |

### 速查表
| 文件 | 说明 |
|------|------|
| [git.md](./速查表/git.md) | Git 命令一页通 |
| [docker.md](./速查表/docker.md) | Docker 命令一页通 |
| [vim.md](./速查表/vim.md) | Vim 快捷键 |
| [命令行.md](./速查表/命令行.md) | CLI 常用命令 |
| [markdown.md](./速查表/markdown.md) | Markdown 语法 |
| [正则表达式.md](./速查表/正则表达式.md) | 正则快速参考 |
| [curl.md](./速查表/curl.md) | curl 常用参数 |
| [systemd.md](./速查表/systemd.md) | systemctl / journalctl |

### AI 效率
| 文件 | 说明 |
|------|------|
| [OpenAI.md](./AI效率/OpenAI.md) | OpenAI API 笔记 |
| [Claude.md](./AI效率/Claude.md) | Anthropic Claude API |
| [国产大模型.md](./AI效率/国产大模型.md) | 通义/智谱/文心/DeepSeek |
| [API-Key管理.md](./AI效率/API-Key管理.md) | API Key 安全存储 |
| [copilot.md](./AI效率/copilot.md) | GitHub Copilot |
| [cursor.md](./AI效率/cursor.md) | Cursor IDE |
| [codeium.md](./AI效率/codeium.md) | Codeium 免费替代 |
| [通义灵码.md](./AI效率/通义灵码.md) | 阿里 AI 编程助手 |
| [AI助手对比.md](./AI效率/AI助手对比.md) | 横向对比选型 |

### 软件推荐
| 文件 | 说明 |
|------|------|
| [mac.md](./软件推荐/mac.md) | macOS 推荐软件 |
| [windows.md](./软件推荐/windows.md) | Windows 推荐软件 |
| [linux.md](./软件推荐/linux.md) | Linux 推荐软件 |
| [跨平台.md](./软件推荐/跨平台.md) | 全平台通用软件 |
| [vscode-扩展推荐.md](./软件推荐/vscode-扩展推荐.md) | VS Code 必装扩展 |

### 附录
| 文件 | 说明 |
|------|------|
| [镜像源.md](./附录/镜像源.md) | pip/npm/docker 镜像汇总 |
| [术语表.md](./附录/术语表.md) | 技术名词解释 |
| [学习资源.md](./附录/学习资源.md) | 优质学习资料推荐 |
| [开源许可证.md](./附录/开源许可证.md) | MIT/GPL/Apache 选择指南 |

---

## 🎯 使用方式

1. **一条龙搭建** — 跟着 [🚀 链路笔记](./学习教程/) 从零到一
2. **按场景查找** — 看上方参考手册，找到你想解决的具体问题
3. **速查命令** — 翻 [速查表](./速查表/) 快速回顾
4. **从头配置** — 开 [新机配置清单](./新机配置/) 按步骤走

---

## 🗺️ 推荐学习路径

```
新操作系统到手
↓
[新机配置] 按清单配置系统
↓
[安装软件] 装包管理器 → Git → 编程语言 → Docker
↓
[配置调优] Git / SSH / WSL / 换源 / 环境变量
↓
[终端美化] 字体 → Shell → Prompt → 插件
↓
[一条龙链路] 
│
├─ 🧠 Python 深度学习链路 → 配置 GPU 训练环境
├─ 🤖 AI 编程链路 → 配置 AI 编程助手
├─ 🌐 Node.js 前后端链路 → 搭建全栈项目
├─ 🐳 Docker 链路 → 容器化部署
└─ 🎨 终端美化链路 → 打造高颜值终端
↓
[速查表] 日常复习
```

---

## 🤝 如何贡献

欢迎补充、修正或添加新内容！

1. Fork 本仓库
2. 创建你的分支
3. 提交更改
4. 推送到分支
5. 提交 Pull Request

详见 [贡献指南](./CONTRIBUTING.md)

---

## 📄 许可证

MIT License — 自由使用、修改、分享

---

## ⭐ 支持一下

如果这个仓库对你有帮助，请点击右上角 **Star** ⭐

你的 Star 是我持续更新的动力！

---

**Happy Coding!** 🚀
