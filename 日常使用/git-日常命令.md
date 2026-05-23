# Git 日常命令

每天都要用的 Git 命令，覆盖完整的工作流。

---

## 初始化 / 克隆

```bash
# 新建仓库
git init

# 克隆远程仓库
git clone git@github.com:user/repo.git
git clone git@github.com:user/repo.git my-folder  # 指定目录
```

---

## 查看状态

```bash
git status                 # 完整状态
git status -s              # 简洁模式（两列状态码）
```

状态码含义：`??` 未跟踪，`M` 已修改，`A` 已暂存。

---

## 暂存区（Staging Area）说明

Git 有三层：工作区 -> 暂存区 -> 仓库。

- 工作区修改文件
- `git add` 把改动放到暂存区（准备提交）
- `git commit` 把暂存区内容写入仓库

好处：可以只提交一部分改动，而不是一次提交所有修改。

---

## 添加文件到暂存区

```bash
git add filename          # 添加单个文件
git add .                 # 添加当前目录所有变化
git add -A                # 添加所有变化（包括删除）
git add -p                # 交互式分段添加（推荐，可以只加部分改动）
```

---

## 查看改动

```bash
git diff                  # 工作区 vs 暂存区（未 add 的改动）
git diff --staged         # 暂存区 vs 上次 commit（已 add 的改动）
git diff HEAD             # 工作区 vs 上次 commit
```

---

## 提交

```bash
git commit -m "feat: add user login API"
git commit -am "fix: typo in readme"   # 跳过 git add（仅跟踪过的文件）
```

如果忘记加文件：

```bash
git add forgotten-file
git commit --amend --no-edit   # 合并到上一个 commit
```

### 提交信息规范（约定式提交）

```
<type>: <简短描述>

feat:     新功能
fix:      修复 bug
docs:     文档
refactor: 重构
test:     测试
chore:    构建/工具
style:    格式（不影响功能）
```

---

## 推送

```bash
git push origin main             # 推送到远程 main
git push -u origin main          # 首次推送，建立跟踪关系
git push --force-with-lease      # 安全强制推送（比 --force 安全）
```

`--force-with-lease` 会在强制推送前检查远程是否有新 commit，避免覆盖别人代码。

---

## 拉取

```bash
git pull                       # 拉取并合并（相当于 fetch + merge）
git pull --rebase              # 拉取并变基（保持线性历史，推荐）
git fetch                      # 仅拉取，不合并
```

推荐用 `git pull --rebase`，避免多余的 merge commit。

设置默认用 rebase：

```bash
git config --global pull.rebase true
```

---

## 查看历史

```bash
git log                       # 完整历史
git log --oneline             # 一行一个 commit
git log --oneline --graph     # 带分支图
git log -5                    # 最近 5 条
git log --oneline --all       # 所有分支
git log --author="name"       # 按作者过滤
git log --since="2 weeks ago" # 按时间过滤
git log -p                    # 显示每次 diff
```

---

## .gitignore 基础

根目录创建 `.gitignore`，每行一个匹配模式：

```
# 依赖
node_modules/
vendor/

# 编译输出
dist/
build/
*.class

# 环境变量
.env
.env.local

# 系统文件
.DS_Store
Thumbs.db
```

可以让 `git status` 干净很多。已经跟踪的文件不受 `.gitignore` 影响，需要用 `git rm --cached` 取消跟踪。

---

## 日常典型工作流

```bash
# 1. 拉取最新代码
git pull --rebase

# 2. 查看当前状态
git status

# 3. 改完代码后查看 diff
git diff

# 4. 暂存并提交
git add -p
git commit -m "feat: your message"

# 5. 推送
git push
```
