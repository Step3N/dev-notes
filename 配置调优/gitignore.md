# .gitignore 编写指南

告诉 Git 哪些文件不要跟踪。不止是 `.gitignore`，还有几种方式控制忽略规则。

---

## 语法速查

| 模式 | 含义 | 例子 |
|------|------|------|
| `file.txt` | 忽略具体文件 | `secret.key` |
| `*.log` | 通配符，匹配所有 .log 文件 | `*.log` |
| `build/` | 忽略 build 目录（及其所有内容） | `dist/`、`node_modules/` |
| `**/__pycache__` | 匹配任意层级的目录 | `**/__pycache__` |
| `/foo` | 只匹配根目录的 foo | `/build` |
| `foo/bar` | 匹配任意层级的 foo/bar | `src/foo/bar`、`test/foo/bar` |
| `!important.log` | 取反，不忽略（跳过前面规则） | `!config/prod.json` |
| `?` | 匹配任意单个字符 | `data_?.csv` |
| `[abc]` | 字符集 | `photo[0-9].jpg` |
| `#` | 注释 | `# 这是注释` |
| `\` | 转义特殊字符 | `\!important.md` |

---

## 常见语言模式

### Python

```gitignore
# Python
__pycache__/
*.py[cod]
*.so
.Python
env/
venv/
.venv/
*.egg-info/
dist/
build/
*.egg
.pytest_cache/
.mypy_cache/
.ruff_cache/
```

### Node.js

```gitignore
# Node
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.env
.env.local
dist/
build/
.next/
.cache/
coverage/
```

### Go

```gitignore
# Go
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
*.out
vendor/
dist/
```

### Java / Kotlin

```gitignore
# Java
target/
*.class
*.jar
*.war
*.nar
*.ear
*.log
.gradle/
build/
!gradle-wrapper.jar
```

### Rust

```gitignore
# Rust
/target/
**/*.rs.bk
Cargo.lock   # 库项目不忽略，应用项目可以忽略
```

### macOS / Windows 系统文件

```gitignore
# macOS
.DS_Store
.AppleDouble
.LSOverride
Icon*
._*

# Windows
Thumbs.db
Desktop.ini
$RECYCLE.BIN/
```

---

## 全局 gitignore

不想每个项目都写系统文件忽略？设一个全局的。

```bash
# 创建全局 gitignore
git config --global core.excludesFile ~/.gitignore_global
```

```bash
# 编辑 ~/.gitignore_global
echo ".DS_Store" >> ~/.gitignore_global
echo "Thumbs.db" >> ~/.gitignore_global
```

以后再也不用在每个 `.gitignore` 里写 `.DS_Store` 和 `Thumbs.db` 了。

---

## .git/info/exclude — 项目专属但不上传

`.gitignore` 会提交到仓库，团队所有人都生效。如果你有**自己独享**的忽略规则（比如 IDE 配置、本地脚本），用这个：

```bash
# 编辑 .git/info/exclude（不会提交到仓库）
echo "local-config.json" >> .git/info/exclude
```

**对比：**

| 方式 | 影响范围 | 会提交？ |
|------|----------|----------|
| `.gitignore` | 全部克隆 | ✅ 会 |
| `.git/info/exclude` | 仅本地 | ❌ 不会 |
| 全局 gitignore | 所有仓库 | ❌ 不会 |

---

## 忽略已经跟踪的文件

文件已经被 Git 跟踪了，写 `.gitignore` 没用。需要先停止跟踪：

```bash
# 停止跟踪但保留文件（最常用）
git rm --cached <file>

# 停止跟踪整个目录
git rm -r --cached <directory>
```

**完整流程示例：**
```bash
# 不小心把 .env 提交了
echo ".env" >> .gitignore
git rm --cached .env
git commit -m "chore: stop tracking .env and add to gitignore"
```

> ⚠️ 如果文件包含敏感信息（密钥、密码），`git rm --cached` **不够**！历史记录里还有。需要配合 `git filter-branch` 或 `bfg repo-cleaner` 彻底清除。如果已经 push，立刻轮换密钥。

---

## 模板参考

别自己写，直接抄：

- GitHub 官方模板合集：https://github.com/github/gitignore
- 在线生成器：https://www.toptal.com/developers/gitignore
- CLI 生成：
  ```bash
  npx gitignore node    # 生成 Node.js 项目的 .gitignore
  npx gitignore python  # 生成 Python 项目的
  ```

---

## 验证规则

不确定某条规则是否生效？用 `git check-ignore`：

```bash
# 检查文件是否被忽略
git check-ignore -v dist/main.js
# 输出会告诉你匹配了哪条规则

# 列出所有被忽略的文件
git status --ignored
```
