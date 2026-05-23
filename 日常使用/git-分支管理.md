# Git 分支管理

分支是 Git 最核心的功能，让你并行开发多个特性互不干扰。

---

## 分支命名规范

```
main        主分支，稳定可用
develop     开发分支，集成分支
feat/xxx    功能分支，如 feat/login
fix/xxx     修复分支，如 fix/nav-bug
hotfix/xxx  紧急修复，从 main 分出
release/x.x 发布准备分支
chore/xxx   构建/工具调整
```

建议用 `/` 分层，Git 会按目录结构展示，方便管理。

---

## 列出分支

```bash
git branch                 # 本地分支（* 表示当前）
git branch -r              # 远程分支
git branch -a              # 所有分支（本地+远程）
git branch -v              # 带最后 commit 信息
```

---

## 创建分支

```bash
# 基于当前 HEAD 创建
git branch feat/login

# 创建并切换
git checkout -b feat/login

# 创建并切换（新版方式，推荐）
git switch -c feat/login

# 基于特定 commit 创建
git branch feat/login <commit-hash>
```

---

## 切换分支

```bash
git checkout main             # 传统方式
git switch main               # 新版方式，语义更清晰

# 创建并切换（等价）
git switch -c feat/login
```

推荐用 `git switch`，职责单一，不容易误操作。

---

## 删除分支

```bash
# 本地
git branch -d feat/login      # 安全删除（已合并）
git branch -D feat/login      # 强制删除（未合并也删）

# 远程
git push origin --delete feat/login
git push origin :feat/login   # 等价，较少用
```

---

## 重命名分支

```bash
# 重命名当前分支
git branch -m new-name

# 重命名指定分支
git branch -m old-name new-name
```

---

## 合并（merge）

```bash
git switch main
git merge feat/login
```

### 快进合并（Fast-forward）

如果 main 从分叉后没有新 commit，Git 直接移动指针，不产生 merge commit。

### 三方合并（3-way merge）

如果 main 和 feat/login 都有新 commit，Git 会创建一个 merge commit 把两条历史合起来。

防止快进（总是产生 merge commit）：

```bash
git merge --no-ff feat/login
```

---

## 变基（rebase）

变基是把当前分支的 commits "移植" 到目标分支的最新位置，历史是一条直线。

```bash
git switch feat/login
git rebase main
```

效果：`feat/login` 的 commits 会在 `main` 的最新 commit 之后重放。

### 交互式变基（squash 合并 commit）

```bash
git rebase -i HEAD~3          # 操作最近 3 个 commit
```

会打开编辑器，每个 commit 前可以指定动作：

```
pick  abc123 feat: first
squash def456 feat: second    # 合并到上一个
squash ghi789 feat: third     # 合并到上一个
# 保存退出后会让你写新的 commit message
```

常用操作：

| 命令 | 作用 |
|------|------|
| `pick` | 保留 |
| `reword` | 改 commit message |
| `squash` | 合并到上一个 |
| `fixup` | 合并并丢弃 message |
| `drop` | 删除 commit |

---

## 处理合并冲突

当 merge 或 rebase 遇到冲突时，Git 会暂停并标记冲突文件。

```bash
# 查看冲突文件
git status

# 手动编辑后标记已解决
git add resolved-file

# 继续合并
git merge --continue    # 或 git commit

# 继续变基
git rebase --continue

# 放弃操作
git merge --abort
git rebase --abort
```

冲突标记格式：

```diff
<<<<<<< HEAD
main 分支的内容
=======
合并进来的内容
>>>>>>> feat/login
```

编辑保留需要的部分，删掉 `<<<<<<`、`======`、`>>>>>>`。

---

## 可视化分支历史

```bash
git log --oneline --graph --all
```

输出：

```
* 5f3e2a1 (HEAD -> main) fix: typo
*   a1b2c3d Merge branch 'feat/login'
|\
| * 9e8d7c6 (feat/login) feat: add login form
| * 8f7e6d5 feat: add login API
* | 7c6b5a4 refactor: nav styles
|/
* 6a5b4c3 Initial commit
```

加几个参数更美观：

```bash
git log --oneline --graph --all --decorate
```

---

## 典型分支工作流

```bash
# 1. 从 main 拉新分支
git switch main
git pull
git switch -c feat/my-feature

# 2. 开发过程中同步 main
git fetch
git rebase origin/main

# 3. 开发完，整理 commit
git rebase -i HEAD~3   # squash 成几个有意义的 commit

# 4. 合并回 main
git switch main
git merge feat/my-feature
git push

# 5. 删除旧分支
git branch -d feat/my-feature
```
