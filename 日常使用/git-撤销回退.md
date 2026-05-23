# Git 撤销与回退

Git 给了你后悔药，但分很多种。搞清楚它们的区别，关键时刻不慌张。

---

## 1. 工作区修改未暂存

改了文件但还没 `git add`，想丢弃改动：

```bash
# 丢弃单个文件
git restore <file>

# 丢弃所有文件
git restore .

# 老式写法（一样的效果）
git checkout -- <file>
```

**验证：**
```bash
git status
# 确认工作区干净
```

> ⚠️ 这个操作**不可恢复**！改动的代码就丢了。如果不确定，先 stash 或备份。

---

## 2. 已暂存（staged）但未提交

已经 `git add` 了，想取消暂存：

```bash
# 取消暂存单个文件
git restore --staged <file>

# 取消暂存所有
git restore --staged .

# 老式写法
git reset HEAD <file>
```

**场景对比：**

| 状态 | 命令 | 效果 |
|------|------|------|
| `git add` 后后悔 | `git restore --staged <file>` | 文件回到工作区，改动还在 |
| `git add` 后连改动都想要丢弃 | `git restore --staged <file>` 再 `git restore <file>` | 彻底还原到上次提交 |

---

## 3. 修改最近一次提交

刚 commit 完，发现少改了文件 / 写错 message：

```bash
# 修改 commit message
git commit --amend -m "新的正确的提交信息"

# 添加漏掉的文件
git add <漏掉的文件>
git commit --amend --no-edit
```

**验证：**
```bash
git log --oneline -3
# 看到最近一条 commit 的 hash/message 已变
```

> ⚠️ `--amend` 会生成新 hash。如果已经 push 到共享分支，**不要 amend**，用 `git revert`。

---

## 4. git reset — 回退提交

针对**尚未推送**的本地提交。

### --soft（最温柔）

```bash
# 撤回最近 1 个 commit，改动留在暂存区
git reset --soft HEAD~1
```

### --mixed（默认）

```bash
# 撤回最近 1 个 commit，改动留在工作区（未暂存）
git reset --mixed HEAD~1
# 或者简写
git reset HEAD~1
```

### --hard（最暴力）

```bash
# 撤回最近 1 个 commit，改动全部丢弃
git reset --hard HEAD~1

# 回退到指定提交
git reset --hard <commit-hash>
```

**验证：**
```bash
git log --oneline -5
# 确认 commit 已消失
```

| 模式 | 工作区 | 暂存区 | 安全吗？ |
|------|--------|--------|----------|
| `--soft` | 不变 | 保留 | ✅ 安全 |
| `--mixed` | 修改保留 | 清空 | ✅ 安全 |
| `--hard` | 丢弃 | 清空 | ❌ 不可逆 |

> 💡 用 `--hard` 前先 `git stash` 或备份，或者记住 hash 还能从 reflog 找回。

---

## 5. git revert — 安全撤销（共享分支）

**已经推送了的提交**，用 `revert` 而不是 `reset`。它会创建一个**新的反向提交**，不改变历史。

```bash
# 撤销某一次提交
git revert <commit-hash>

# 撤销连续多个（逐个弹窗编辑 message）
git revert HEAD~3..HEAD

# 不弹窗直接 revert
git revert --no-edit <commit-hash>
```

**验证：**
```bash
git log --oneline -5
# 看到新增了一条 "Revert ..." 的 commit
```

**场景：**

| 场景 | 用 reset？ | 用 revert？ |
|------|-----------|-------------|
| 本地 commit，没 push | ✅ `reset --mixed` | ❌ 没必要 |
| 已 push 到个人分支 | ✅ 但需要 force push | ✅ 更安全 |
| 已 push 到 main / 共享分支 | ❌ 会搞死队友 | ✅ **必须用 revert** |

---

## 6. git reflog — 最后的救命稻草

`git reset --hard` 搞砸了？`git rebase` 翻车了？`reflog` 记录了你**所有 HEAD 移动历史**。

```bash
# 查看操作历史
git reflog

# 输出示例：
# e3a1b2c (HEAD -> main) HEAD@{0}: reset: moving to HEAD~1
# f4g5h6i HEAD@{1}: commit: 加了个功能
# a7b8c9d HEAD@{2}: commit: 修了个 bug

# 恢复到任意历史位置
git reset --hard HEAD@{1}
```

> 💡 `reflog` 只存**本地**操作记录，不会 push 到远程。默认 90 天过期。

---

## 速查表：该用哪个？

| 你想做什么 | 命令 |
|-----------|------|
| 丢弃工作区改动 | `git restore <file>` |
| 取消暂存 | `git restore --staged <file>` |
| 改上次 commit 内容 | `git commit --amend` |
| 改上次 commit message | `git commit --amend -m "新信息"` |
| 撤回本地 commit，保留改动 | `git reset --mixed HEAD~1` |
| 撤回本地 commit，彻底丢弃 | `git reset --hard HEAD~1` |
| 已推送，想安全撤回 | `git revert <commit-hash>` |
| 找回了误删的 commit | `git reflog` + `git reset --hard` |
