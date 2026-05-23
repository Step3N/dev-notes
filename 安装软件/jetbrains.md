# JetBrains 全家桶

JetBrains 为每种主流语言提供了专用 IDE，共享同一套底层平台（IntelliJ Platform），所以快捷键、UI、调试体验高度一致。

---

## 产品一览

| IDE | 语言 | 特色 |
|-----|------|------|
| IntelliJ IDEA | Java, Kotlin, 多语言 | 最强大的 Java IDE，支持 Spring、Android |
| PyCharm | Python | 支持 Django、Jupyter、科学计算 |
| WebStorm | JavaScript / TypeScript | React、Vue、Angular、Node.js |
| GoLand | Go | 调试、覆盖率、Go Module 深度支持 |
| CLion | C / C++ | CMake 集成、调试器、静态分析 |
| RustRover | Rust | 2017 年起官方支持的 Rust IDE |
| DataGrip | SQL | 多数据库管理、可视化查询 |
| RubyMine | Ruby / Rails | — |
| PhpStorm | PHP | — |

---

## 安装：JetBrains Toolbox（推荐）

Toolbox App 统一管理所有 IDE 的安装、升级、版本切换、项目打开。

**macOS**

```bash
brew install --cask jetbrains-toolbox
```

**Windows**

```powershell
winget install JetBrains.Toolbox
```

**Linux**

```bash
# 从官网下载 Toolbox App：https://jetbrains.com/toolbox
# 下载后解压运行
tar -xzf jetbrains-toolbox-*.tar.gz
./jetbrains-toolbox
```

安装后打开 Toolbox → 选择 IDE → Install → 点击即可启动。

> 也可以直接从官网下载 IDE 独立安装包，但 Toolbox 极大简化了多 IDE 的管理。

---

## 核心功能

### 智能自动补全

- 不依赖索引即可基础补全
- 索引完成后提供**深度补全**（类型推导、链式调用、框架感知）
- `⌘Space` / `Ctrl+Space` 触发补全

### 重构

| 重构操作 | macOS | Windows / Linux |
|----------|-------|-----------------|
| 重命名 | `⇧F6` | `Shift+F6` |
| 提取变量 | `⌘⌥V` | `Ctrl+Alt+V` |
| 提取方法 | `⌘⌥M` | `Ctrl+Alt+M` |
| 改变签名 | `⌘F6` | `Ctrl+F6` |

### 调试

- 行内断点、条件断点、表达式计算
- 评估表达式（`⌥F8` / `Alt+F8`）
- 单步执行、Step Into / Step Out

### 版本控制

- 内置 Git 支持：diff、merge、commit、history、branches
- GitHub / GitLab PR 集成

---

## 配置同步

### JetBrains Account（推荐）

`Settings` → `Settings Sync` → 登录 JetBrains Account → 开启同步。

自动同步：快捷键、配色、代码风格、插件列表。

### Settings Repository（Git）

`File` → `Settings Repository` → 输入 Git 仓库 URL → Overwrite Local / Merge。

适合团队共享配置。

---

## 每语言必备插件

| IDE | 推荐插件 |
|-----|----------|
| 通用 | **Key Promoter X**（提示快捷键）、**Rainbow Brackets**、**GitToolBox** |
| IntelliJ IDEA | Lombok、Maven Helper、CheckStyle-IDEA |
| PyCharm | Pytest 已内置；可加 .env files support |
| WebStorm | Prettier（内置）、ESLint（内置）、GitLens-like 功能已内置 |
| GoLand | Go Template 支持（内置）、Protocol Buffers |
| CLion | CMake Simple Highlighter、Native Debugging（内置） |
| RustRover | Toml（已内置）、GitHub Copilot |

---

## 关键快捷键

无论哪个 IDE，下面这组快捷键都通用：

| 操作 | macOS | Windows / Linux |
|------|-------|-----------------|
| 查找所有动作 | `⌘⇧A` | `Ctrl+Shift+A` |
| 查找文件 | `⌘⇧O` | `Ctrl+Shift+N` |
| 搜索全局 | `⌘⇧F` | `Ctrl+Shift+F` |
| 跳转到定义 | `⌘B` | `Ctrl+B` |
| 最近文件 | `⌘E` | `Ctrl+E` |
| 工程窗口 | `⌘1` | `Alt+1` |
| 全局替换 | `⌘⇧R` | `Ctrl+Shift+R` |
| 格式化代码 | `⌘⌥L` | `Ctrl+Alt+L` |
| 终端 | `⌥F12` | `Alt+F12` |

> 💡 **`⌘⇧A` / `Ctrl+Shift+A`（Find Action）是最重要的快捷键** — 当你不知道某个功能在哪时，用它搜索。

---

## 性能调优

### 增加内存

编辑 IDE 的 VM Options（`Help` → `Edit Custom VM Options`）：

```txt
-Xms2048m
-Xmx4096m
```

不要超过物理内存的一半。`Xms` 是初始堆，`Xmx` 是最大堆。

### 其他调优

- **关闭不需要的插件**：`Settings` → `Plugins` → 禁用不用的语言/框架
- **Power Save Mode**：`File` → `Power Save Mode`，省电场景下关闭大量索引
- **增大索引缓存**：`Help` → `Change Memory Settings`

---

## 平台差异

| 项目 | macOS | Windows | Linux |
|------|-------|---------|-------|
| 配置目录 | `~/Library/Application Support/JetBrains/` | `%APPDATA%\JetBrains\` | `~/.config/JetBrains/` |
| 缓存目录 | `~/Library/Caches/JetBrains/` | `%LOCALAPPDATA%\JetBrains\` | `~/.cache/JetBrains/` |
| VM 选项编辑 | `⌘⇧A` → `VM Options` | `Ctrl+Shift+A` → `VM Options` | 同 Windows |

---

## 🔗 参考

- JetBrains Toolbox: https://www.jetbrains.com/toolbox-app
- 快捷键 PDF（可下载）: https://www.jetbrains.com/help/idea/mastering-keyboard-shortcuts.html
