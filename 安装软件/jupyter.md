# Jupyter 安装与使用

> 交互式编程环境，适合数据分析、机器学习实验和代码探索

**适用平台**：macOS / Windows / Linux

## 安装

### 基础安装

```bash
# 在 conda 环境或虚拟环境中安装
pip install jupyter

# 或者安装 Jupyter Lab（推荐，更现代的界面）
pip install jupyterlab
```

### conda 环境安装

```bash
conda install jupyter jupyterlab -c conda-forge
```

### 验证

```bash
jupyter --version
# 预期: JupyterLab x.x.x 等
```

## 启动

```bash
# 启动 Jupyter Notebook
jupyter notebook

# 启动 Jupyter Lab（推荐）
jupyter lab
```

默认在 `http://localhost:8888` 打开浏览器。

## 常用配置

### 设置密码（远程访问）

```bash
jupyter notebook password
# 输入密码，会保存到 ~/.jupyter/jupyter_server_config.json
```

### 配置文件

```bash
jupyter notebook --generate-config
# 生成 ~/.jupyter/jupyter_notebook_config.py
```

常用配置项：
```python
c = get_config()
c.ServerApp.port = 8888
c.ServerApp.open_browser = False           # 不自动打开浏览器
c.ServerApp.allow_remote_access = True     # 允许远程访问
c.ServerApp.ip = '0.0.0.0'                # 监听所有网卡
```

### 内核管理

Jupyter 可以支持多种语言内核（Python / R / Julia 等）。

```bash
# 查看已安装的内核
jupyter kernelspec list

# 安装 Python 内核（通常已自带）
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"

# 为 conda 环境注册内核
conda activate myenv
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

这样可以在 Jupyter 中切换不同 conda 环境。

### VS Code 集成

直接在 VS Code 中打开 `.ipynb` 文件即可使用 Jupyter Notebook 功能。
需要在 VS Code 中安装 Jupyter 扩展。

## 常用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Shift+Enter` | 运行当前单元格并选中下一个 |
| `Ctrl+Enter` | 运行当前单元格 |
| `A` | 在上方插入新单元格 |
| `B` | 在下方插入新单元格 |
| `DD` | 删除当前单元格 |
| `M` | 切换到 Markdown 模式 |
| `Y` | 切换到 Code 模式 |

## 常见问题

| 问题 | 解决 |
|------|------|
| 端口 8888 被占用 | `jupyter lab --port 8889` |
| conda 环境的内核找不到 | 在对应环境中执行 `python -m ipykernel install --user --name 环境名` |
| Jupyter 启动后无法访问 | 检查防火墙，配置 `--ip=0.0.0.0` |
| 内核崩溃 | 运行 `jupyter kernelspec list` 检查内核路径，重新安装 ipykernel |

📝 **更新记录**
| 日期 | 更新内容 |
|------|----------|
| 2026-05-23 | 初始版本 |
