# npm 换源配置

在国内使用 npm 官方源很慢，换为国内镜像可以显著提升安装速度。

---

## 🚀 快速换源

### 使用淘宝镜像（推荐）

```bash
npm config set registry https://registry.npmmirror.com
```

验证：

```bash
npm config get registry
# 输出: https://registry.npmmirror.com
```

恢复官方源：

```bash
npm config set registry https://registry.npmjs.org
```

### 其他国内镜像

```bash
# 华为云
npm config set registry https://mirrors.huaweicloud.com/repository/npm/

# 腾讯云
npm config set registry https://mirrors.cloud.tencent.com/npm/

# 中科大
npm config set registry https://npmreg.mirrors.ustc.edu.cn/
```

---

## 📝 配置文件（.npmrc）

npm 配置存储在 `.npmrc` 文件中。按优先级从高到低：

| 级别 | 路径 |
|------|------|
| 项目级 | `./.npmrc`（当前项目根目录） |
| 用户级 | `~/.npmrc`（macOS/Linux） / `C:\Users\<用户名>\.npmrc`（Windows） |
| 全局级 | `$PREFIX/etc/npmrc` |
| 内置 | npm 自带的默认配置 |

查看所有配置来源：

```bash
npm config list
```

### 项目级换源（推荐）

在项目根目录创建 `.npmrc`：

```
registry=https://registry.npmmirror.com
```

> 项目级配置仅对该项目生效，不影响全局。适合团队统一使用镜像。

### 同时配置多个参数

```bash
# 写入 ~/.npmrc
npm config set registry https://registry.npmmirror.com
npm config set save-exact true         # 锁定精确版本
npm config set audit false             # 关闭安全审计（加速）
```

---

## 🛠 cnpm — 替代客户端

淘宝镜像的专用客户端，自动使用国内源：

```bash
npm install -g cnpm

# 使用
cnpm install express
cnpm install -g vite
```

> cnpm 安装包会从淘宝镜像下载，但**不完全兼容 npm**（如 lockfile 格式），优先推荐直接用 `npm config set registry`。

---

## 🔄 nrm — 镜像源管理工具

```bash
npm install -g nrm
```

常用命令：

```bash
# 列出所有源
nrm ls

# 输出示例:
#   npm ---------- https://registry.npmjs.org/
#   yarn --------- https://registry.yarnpkg.com/
#   tencent ------ https://mirrors.cloud.tencent.com/npm/
#   cnpm --------- https://r.cnpmjs.org/
# * taobao ------- https://registry.npmmirror.com/
#   npmMirror ---- https://skimdb.npmjs.com/registry/

# 切换源
nrm use taobao

# 测试速度
nrm test

# 添加自定义源
nrm add company https://registry.company.com/npm/

# 删除源
nrm delete company
```

---

## ⚠️ 常见问题

### 换源后发布包失败

```bash
# 发布时需要临时切换回官方源
npm config set registry https://registry.npmjs.org
npm publish
```

或使用发布参数：

```bash
npm publish --registry=https://registry.npmjs.org
```

### 部分包仍有二进制文件下载慢

即使 npm 源换了，有些包的二进制文件（如 `node-sass`、`sharp`）从 GitHub Releases 下载，仍需配二进制镜像：

```bash
# node-sass
npm config set sass_binary_site https://npmmirror.com/mirrors/node-sass/

# sharp
npm config set sharp_binary_host https://npmmirror.com/mirrors/sharp/
npm config set sharp_libvips_binary_host https://npmmirror.com/mirrors/sharp-libvips/

# electron
npm config set electron_mirror https://npmmirror.com/mirrors/electron/

# puppeteer
npm config set puppeteer_download_host https://npmmirror.com/mirrors/
```

### 查看当前使用了哪个源

```bash
npm config get registry
```

---

## 🔗 参考

- npmmirror: https://npmmirror.com
- nrm: https://github.com/Pana/nrm
