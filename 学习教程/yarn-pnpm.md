# Yarn & pnpm — npm 的替代方案

npm 的替代包管理器，各有优势，适合不同场景。

---

## 🧶 Yarn

Yarn（Classic v1）由 Facebook 创建，主打**速度快、锁文件可靠**。

### 安装

```bash
# 通过 npm（全平台）
npm install -g yarn

# macOS
brew install yarn

# Windows
winget install Yarn.Yarn
```

验证：

```bash
yarn --version
```

### 常用命令对照

| 操作 | npm | yarn |
|------|-----|------|
| 初始化 | `npm init` | `yarn init` |
| 安装依赖 | `npm install` | `yarn install` |
| 添加依赖 | `npm install <pkg>` | `yarn add <pkg>` |
| 移除依赖 | `npm uninstall <pkg>` | `yarn remove <pkg>` |
| 更新依赖 | `npm update <pkg>` | `yarn upgrade <pkg>` |
| 全局安装 | `npm install -g <pkg>` | `yarn global add <pkg>` |
| 运行脚本 | `npm run <script>` | `yarn <script>` |

### 关键特性

- **yarn.lock** — 确定性锁文件，保证各环境依赖一致
- **离线缓存** — 已下载过的包无需再次下载
- **并行安装** — 比 npm 串行安装更快

### 迁移从 npm

```bash
# 已有 package-lock.json 的项目
yarn import
# 将 package-lock.json 转换为 yarn.lock
```

---

## 📦 pnpm

pnpm 主打**磁盘效率**：使用硬链接 + 符号链接，多项目共享同一份依赖。

### 安装

```bash
# 通过 npm（全平台）
npm install -g pnpm

# macOS
brew install pnpm

# Windows
winget install pnpm
```

验证：

```bash
pnpm --version
```

### 常用命令

```bash
pnpm init                    # 初始化
pnpm install                 # 安装所有依赖
pnpm add express              # 添加依赖
pnpm add -D typescript        # 添加 dev 依赖
pnpm remove express           # 移除
pnpm update                   # 更新
pnpm run dev                  # 运行脚本
pnpm audit                    # 安全审计
pnpm outdated                 # 查看过期的包
```

### 关键特性

- **节省磁盘** — 所有包存储在全局 store，项目通过硬链接引用
- **严格** — 默认 `--shamefully-hoist=false`，只能引用 `package.json` 中声明的包
- **快** — 安装速度比 npm/yarn 快 2-3 倍

迁移从 npm：

```bash
# 在已有项目中
pnpm import
# 从 package-lock.json / yarn.lock 生成 pnpm-lock.yaml

# 然后删除 node_modules
rm -rf node_modules
pnpm install
```

---

## 🔍 对比总结

| 特性 | npm | Yarn (v1) | pnpm |
|------|-----|-----------|------|
| 锁文件 | `package-lock.json` | `yarn.lock` | `pnpm-lock.yaml` |
| 安装速度 | 慢 | 中等 | 快 |
| 磁盘占用 | 高 | 高 | 低（硬链接） |
| 严格性 | 松散 | 松散 | 严格 |
| 插件支持 | 内置 | 需插件 | 内置 |
| Workspaces | 支持 | 支持 | 支持（最成熟） |
| 生态系统 | 最广泛 | 活跃 | 快速增长 |

---

## ✅ 选型建议

| 场景 | 推荐 |
|------|------|
| 单项目、小团队 | npm（自带，无需额外安装） |
| 需要确定性构建 | Yarn v1（lockfile 成熟） |
| 多项目、CI/CD、磁盘有限 | **pnpm**（节省空间，严格执行） |
| monorepo 项目 | pnpm（workspace 支持最好） |

---

## 💡 使用国内镜像

两种工具也支持 registry 配置：

```bash
# yarn
yarn config set registry https://registry.npmmirror.com

# pnpm
pnpm config set registry https://registry.npmmirror.com
```

验证：

```bash
yarn config get registry
pnpm config get registry
```

---

## 🔗 参考

- Yarn: https://yarnpkg.com
- pnpm: https://pnpm.io
- 迁移工具: https://pnpm.io/cli/import
