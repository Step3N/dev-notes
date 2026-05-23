# Python 虚拟环境

虚拟环境隔离项目依赖，避免不同项目之间包版本冲突。

---

## venv（Python 内置，推荐基础使用）

Python 3.3+ 自带，无需额外安装。

```bash
# 创建虚拟环境
python3 -m venv .venv

# macOS / Linux 激活
source .venv/bin/activate

# Windows PowerShell 激活
.venv\Scripts\Activate.ps1

# Windows CMD 激活
.venv\Scripts\activate.bat

# 验证
which python   # macOS/Linux — 指向 .venv 内
where python   # Windows — 指向 .venv 内

# 安装包
pip install requests

# 退出
deactivate
```

**最佳实践**：

- `.venv` 作为目录名（通用约定，已在 `.gitignore` 中常见）
- 不要提交虚拟环境目录到 git
- 配合 `requirements.txt` 锁定依赖：
  ```bash
  pip freeze > requirements.txt
  pip install -r requirements.txt
  ```

---

## virtualenvwrapper（方便管理多个环境）

```bash
# 安装
pip install virtualenvwrapper

# macOS / Linux 配置（加到 ~/.zshrc）
export WORKON_HOME=$HOME/.virtualenvs
source $(which virtualenvwrapper.sh)

# Windows
pip install virtualenvwrapper-win
```

```bash
# 创建环境
mkvirtualenv myproject

# 切换环境
workon myproject

# 退出
deactivate

# 列出所有环境
lsvirtualenv

# 删除环境
rmvirtualenv myproject
```

> 适合需要频繁创建/切换多个环境的场景。但项目自身不显式声明环境，可移植性不如 venv + requirements.txt。

---

## Poetry（现代依赖管理）

安装：

```bash
# macOS / Linux
curl -sSL https://install.python-poetry.org | python3 -

# Windows PowerShell
(Invoke-WebRequest -Uri https://install.python-poetry.org -UseBasicParsing).Content | python -
```

```bash
# 验证
poetry --version

# 初始化新项目
poetry new myproject
cd myproject

# 或在已有项目中初始化
poetry init

# 安装依赖
poetry add requests

# 安装所有依赖（基于 pyproject.toml / poetry.lock）
poetry install

# 进入虚拟环境
poetry shell

# 退出
exit
```

优势：
- 自动管理虚拟环境
- `pyproject.toml` 统一项目配置
- 锁文件 `poetry.lock` 确保依赖一致性
- 内置构建和发布功能

---

## Conda（数据科学首选）

Conda 是 Anaconda/Miniconda 自带的环境管理器，**特别适合数据科学和深度学习场景**，因为它能管理非 Python 依赖（如 CUDA、cuDNN）。

安装：下载 [Miniconda](https://docs.anaconda.com/miniconda/)（轻量）或 Anaconda（全量）。

```bash
# 创建环境
conda create -n myenv python=3.12

# 激活 / 退出
conda activate myenv
conda deactivate

# 安装包
conda install numpy pandas
# 或指定 channel 安装非 Python 依赖
conda install pytorch pytorch-cuda=12.4 -c pytorch -c nvidia

# 导出 / 导入环境（可复现的关键）
conda env export > environment.yml
conda env create -f environment.yml

# 删除环境
conda remove -n myenv --all
conda env list  # 确认已删除

# 指定 Python 版本创建
conda create -n py311 python=3.11
conda create -n py310 python=3.10 numpy pandas
```

优势：预编译二进制包，解决 C 扩展编译问题；自带科学计算生态。

---

## 对比总结

| 工具 | 适用场景 | 是否管理 Python 版本 | 是否管理非 Python 包 | 速度 | 推荐场景 |
|------|---------|-------------------|-------------------|------|---------|
| **venv** | 通用，小型项目 | ❌ | ❌ | ⚡ 快 | 日常 Python 项目 |
| **virtualenvwrapper** | 多环境频繁切换 | ❌ | ❌ | ⚡ 快 | 需要频繁创建/切换环境 |
| **Poetry** | 库/项目开发 | ❌ | ❌ | ⚡ 快 | 发布库、需要锁文件的项目 |
| **Conda** | 数据科学/ML | ✅ | ✅ | 🐢 较慢（依赖解析） | 数据科学、深度学习、跨语言项目 |

> **推荐策略**：日常项目用 **venv**，数据科学用 **Conda**，发布库用 **Poetry**。不建议在一个项目里混用多个管理工具。
