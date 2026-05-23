# pip 换源配置

在中国大陆访问 PyPI 官方源速度很慢，配置国内镜像可以显著提升下载速度。

---

## 常用国内镜像

| 镜像站 | URL |
|--------|-----|
| 清华 TUNA | `https://pypi.tuna.tsinghua.edu.cn/simple` |
| 阿里云 | `https://mirrors.aliyun.com/pypi/simple/` |
| 中科大 USTC | `https://pypi.mirrors.ustc.edu.cn/simple/` |
| 腾讯云 | `https://mirrors.cloud.tencent.com/pypi/simple/` |
| 华为云 | `https://repo.huaweicloud.com/repository/pypi/simple/` |

---

## 全局配置

### 命令行一键设置（推荐）

```bash
# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# 阿里云
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/

# 中科大
pip config set global.index-url https://pypi.mirrors.ustc.edu.cn/simple/
```

验证：

```bash
pip config list
pip config debug
```

### 配置文件位置

`pip config set` 本质是修改配置文件。文件路径：

| 平台 | 用户级配置文件 |
|------|---------------|
| **macOS / Linux** | `~/.config/pip/pip.conf` |
| **Windows** | `%APPDATA%\pip\pip.ini` |

也可以手动创建：

```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

---

## 项目级配置

在项目根目录创建 `pip.conf`（macOS/Linux）或 `pip.ini`（Windows）：

```bash
# 在项目目录下创建
touch pip.conf
```

```ini
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

项目级配置的优先级高于全局配置。

---

## 一次性使用（不修改配置）

```bash
pip install requests -i https://pypi.tuna.tsinghua.edu.cn/simple
```

配合 `--trusted-host`（如果镜像使用 HTTP 而非 HTTPS）：

```bash
pip install requests -i http://mirrors.aliyun.com/pip/simple/ --trusted-host mirrors.aliyun.com
```

> ⚠️ 阿里云镜像使用 HTTP 而非 HTTPS。内网环境无风险，公网请使用 HTTPS 镜像（清华、华为云）。

## 多源备用配置

如果某个镜像挂了，配置多个镜像源：

```bash
# 清华为主，阿里为备用
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip config set global.extra-index-url https://mirrors.aliyun.com/pypi/simple/
```

---

## 恢复官方源

```bash
pip config unset global.index-url
```

或直接删除配置文件中的 `index-url` 行。

---

## 验证下载速度

```bash
# 创建一个测试用的空环境或用 --dry-run
pip install numpy --dry-run -v 2>&1 | grep "Looking in indexes"
```

或者直接对比下载时间：

```bash
# 先看当前源
pip config list

# 测试下载一个较大的包
time pip install pandas -q
```

> **提示**：清华源同步频率最高（5 分钟），阿里云在国内下载速度最快。建议优先选清华源。
