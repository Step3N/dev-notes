# GitHub Actions CI/CD 基础

## 核心概念

| 概念 | 说明 |
|------|------|
| **Workflow** | 一个自动化流程，定义在 `.github/workflows/*.yml` 中 |
| **Job** | Workflow 内的一组步骤，默认并行执行 |
| **Step** | Job 内的单个任务（运行命令或使用 Action） |
| **Action** | 可复用的 GitHub 操作（来自 Marketplace 或自定义） |
| **Runner** | 运行 Job 的虚拟机（GitHub 托管或自托管） |
| **Event** | 触发 Workflow 的事件（push、PR、schedule 等） |

目录结构：

```
.github/
└── workflows/
    ├── ci.yml
    ├── deploy.yml
    └── release.yml
```

## 基础 Workflow

```yaml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      - name: Run tests
        run: pytest
```

触发条件：任何 push 或 PR 到任意分支时执行。

## 常用触发事件（on）

```yaml
on:
  push:
    branches: [main, develop]       # 仅特定分支
    paths: ['src/**', '!docs/**']   # 仅特定路径变更
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 6 * * 1'            # 每周一 06:00 UTC
  workflow_dispatch:                 # 手动触发（Actions 页面点按钮）
  release:
    types: [published]               # 发布 Release 时触发
```

## Matrix 构建：多版本测试

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        python-version: ['3.10', '3.11', '3.12']
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - run: pip install -r requirements.txt
      - run: pytest
```

等同于 3×3 = 9 个并行 Job。排除特定组合：

```yaml
matrix:
  os: [ubuntu-latest, windows-latest]
  node: [18, 20, 22]
  exclude:
    - os: windows-latest
      node: 22
```

## 常用 Actions

| Action | 用途 |
|--------|------|
| `actions/checkout@v4` | 检出代码 |
| `actions/setup-python@v5` | 安装指定 Python 版本 |
| `actions/setup-node@v4` | 安装指定 Node 版本 |
| `actions/cache@v4` | 缓存依赖加速构建 |
| `actions/upload-artifact@v4` | 上传构建产物 |
| `actions/download-artifact@v4` | 下载构建产物 |
| `docker/login-action@v3` | 登录 Docker Registry |
| `docker/build-push-action@v6` | 构建并推送 Docker 镜像 |

## 缓存依赖

```yaml
- name: Cache pip
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
    restore-keys: |
      ${{ runner.os }}-pip-
```

Node.js 缓存：

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

## Secrets（敏感信息）

**设置**：仓库 → Settings → Secrets and variables → Actions

**使用**：

```yaml
- name: Deploy
  env:
    DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
    API_TOKEN: ${{ secrets.API_TOKEN }}
  run: |
    echo "$DEPLOY_KEY" > deploy_key
    chmod 600 deploy_key
    rsync -e "ssh -i deploy_key" -avz ./ user@host:/var/www/
```

> 永远不要将敏感信息明文写在 YAML 中。Secrets 在日志中会自动遮蔽。

## 完整实战：Python 包 CI + 发布到 PyPI

```yaml
name: Python Package

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  release:
    types: [published]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install ruff
      - run: ruff check .

  test:
    needs: lint
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
        python-version: ['3.10', '3.11', '3.12']
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('**/pyproject.toml') }}
      - run: pip install -e ".[dev]"
      - run: pytest --cov

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install build
      - run: python -m build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  publish:
    if: github.event_name == 'release' && github.event.action == 'published'
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: pypi
      url: https://pypi.org/p/your-package
    permissions:
      id-token: write
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/
      - uses: pypa/gh-action-pypi-publish@release/v1
```

## 本地调试

安装 [act](https://github.com/nektos/act) 在本地运行 Actions：

```bash
# macOS
brew install act

# Linux
curl -s https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# 运行 push 事件
act push

# 指定 Runner 镜像
act -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

## 快速验证

```bash
# 检查 YAML 语法
pip install yamllint
yamllint .github/workflows/*.yml
```

> **平台差异**：GitHub 托管的 Runner 支持 ubuntu-latest / macos-latest / windows-latest。所有命令在对应平台的原生 shell 中执行（Linux/macOS 用 bash，Windows 用 pwsh）。自托管 Runner 全平台均可。Workflow 语法完全一致。
