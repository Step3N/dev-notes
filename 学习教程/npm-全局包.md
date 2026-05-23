# 常用全局 npm 包

推荐一些实用的全局工具包，一行安装，全平台通用。

---

## 🌐 开发服务器

### serve — 零配置静态文件服务器

```bash
npm install -g serve
```

使用：

```bash
serve .                  # 启动当前目录
serve -s dist            # SPA 模式（处理 history API fallback）
serve -l 5000            # 指定端口
```

### http-server — 轻量级 HTTP 服务器

```bash
npm install -g http-server
```

使用：

```bash
http-server . -p 8080 -c-1   # 关闭缓存，方便开发
http-server . --cors          # 开启 CORS
```

---

## 🏗️ 项目脚手架

### create-vite — Vite 脚手架（推荐）

```bash
npm create vite@latest
```

或指定模板：

```bash
npm create vite@latest my-app -- --template react-ts
```

### create-react-app — React 脚手架

```bash
npm install -g create-react-app
npx create-react-app my-app
```

> 官方已推荐 Vite 替代 CRA，新项目建议用 `create-vite`。

### @vue/cli — Vue 脚手架

```bash
npm install -g @vue/cli
vue create my-app
```

> Vue 3 官方也推荐 Vite，可用 `npm create vue@latest`。

### @angular/cli — Angular 脚手架

```bash
npm install -g @angular/cli
ng new my-app
```

---

## 🔧 开发工具

### nodemon — 自动重启

监听文件变更，自动重启 Node 进程：

```bash
npm install -g nodemon
nodemon server.js
```

### eslint — 代码检查（Linter）

```bash
npm install -g eslint
eslint --init             # 初始化配置
eslint src/               # 检查文件
eslint src/ --fix         # 自动修复
```

### prettier — 代码格式化

```bash
npm install -g prettier
prettier --write src/     # 格式化整个目录
prettier --check src/     # 仅检查
```

### typescript — TypeScript 编译器

```bash
npm install -g typescript
tsc --version
tsc --init                # 生成 tsconfig.json
tsc                       # 编译
```

---

## 📦 包管理增强

### npm-check-updates — 批量更新依赖

检查并更新 `package.json` 中的依赖版本：

```bash
npm install -g npm-check-updates
ncu                       # 查看可更新
ncu -u                    # 更新 package.json
npm install               # 安装新版本
```

### rimraf — 跨平台删除（rm -rf）

`rm -rf` 的跨平台替代（Windows 友好）：

```bash
npm install -g rimraf
rimraf node_modules
rimraf dist
```

### cross-env — 跨平台环境变量

```bash
npm install -g cross-env
cross-env NODE_ENV=production node server.js
```

> Windows 上 `NODE_ENV=production` 无效，cross-env 解决这个问题。

---

## ✅ 验证已安装的全局包

```bash
npm list -g --depth=0
```

输出示例：

```
/opt/homebrew/lib
├── corepack
├── create-vite
├── eslint
├── http-server
├── nodemon
├── npm
├── prettier
├── rimraf
├── serve
├── typescript
└── yarn
```

---

## 💡 提示

- 全局包尽量精简，优先用 `npx`（免安装执行）
- 团队项目建议将工具作为 devDependencies 本地安装
- 如遇权限错误，macOS/Linux 用 `sudo npm install -g` 或用 nvm 管理

---

## 🔗 参考

- serve: https://github.com/vercel/serve
- http-server: https://github.com/http-party/http-server
- create-vite: https://vitejs.dev
- nodemon: https://nodemon.io
