# Git 高级技巧

日常够用之外的实用技能，能帮你省大量时间。

---

## 1. git stash — 暂存工作现场

做到一半突然要切分支？先 stash 起来。

```bash
# 暂存当前改动
git stash push -m "WIP: 登录功能做了 80%"

# 查看 stash 列表
git stash list
# stash@{0}: On main: WIP: 登录功能做了 80%
# stash@{1}: On main: 修复样式

# 恢复最近的 stash 并删除
git stash pop

# 恢复指定的 stash
git stash apply stash@{1}

# 删除某个 stash
git stash drop stash@{1}

# 清空所有 stash
git stash clear
```

**从 stash 创建分支（最实用）：**
```bash
git stash branch 新分支名 stash@{0}
# 会自动切新分支并把 stash 的内容 apply 上去
```

> 💡 stash 默认不暂存**未跟踪文件**，加 `-u` 或 `--include-untracked` 包含它们。

---

## 2. git reflog — 操作日志

所有"翻车"后的救命工具。

```bash
# 查看所有 HEAD 移动记录
git reflog

# 恢复误删的提交
git reflog
# 找到丢失的 commit hash
git checkout <commit-hash>
# 或从那里创建分支
git branch 救回来的分支 <commit-hash>
```

> 💡 即使 `git branch -D` 删了分支，commit 还在 reflog 里（只要没被 gc）。

---

## 3. git bisect — 二分查找 bug

在几十个 commit 里找 bug 从哪来？`bisect` 帮你自动二分。

```bash
# 开始 bisect
git bisect start

# 标记当前有 bug
git bisect bad

# 标记某个旧版本是好的
git bisect good <已知好版本的 hash 或 tag>

# Git 会 checkout 中间的一个 commit，你测试后标记：
git bisect good   # 这个版本没问题
git bisect bad    # 这个版本有 bug

# 重复几轮后 Git 告诉你第一个 bad commit
# 结束 bisect
git bisect reset
```

**自动化 bisect（配合脚本）：**
```bash
git bisect start HEAD v1.0
git bisect run npm test
# 自动二分运行测试，找到第一个失败的 commit
```

---

## 4. git hooks — 自动化钩子

在 `.git/hooks/` 目录下放可执行脚本，Git 会在特定事件触发时执行。

### pre-commit — 提交前自动检查

```bash
# 创建 pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
# 在提交前运行 linter
npx eslint .
if [ $? -ne 0 ]; then
  echo "❌ ESLint 没通过，提交被阻止"
  exit 1
fi
EOF

chmod +x .git/hooks/pre-commit
```

> ⚠️ `.git/hooks/` 不会提交到仓库。团队共享用 [husky](https://typicode.github.io/husky/)（Node 项目）或 [pre-commit](https://pre-commit.com)（Python 项目）。

**常用 hooks：**

| Hook | 触发时机 | 用途 |
|------|----------|------|
| `pre-commit` | `git commit` 前 | lint、格式化、安全检查 |
| `commit-msg` | 输入 commit message 后 | 检查 message 格式 |
| `pre-push` | `git push` 前 | 运行测试、构建检查 |
| `post-checkout` | `git checkout` 后 | 自动安装依赖 |

---

## 5. git worktree — 同时开多个分支

不需要切分支，把另一个分支 checkout 到别的目录。

```bash
# 把 feature/login 分支 checkout 到 ../login-dir
git worktree add ../login-dir feature/login

# 列出所有 worktree
git worktree list

# 用完删除
git worktree remove ../login-dir
```

> 💡 适合同时改多个分支的场景（比如边改 bug 边开发功能），互不干扰。

---

## 6. git cherry-pick — 挑拣提交

把某个分支的特定 commit 复制到当前分支。

```bash
# 把其他分支的一个 commit 拿过来
git cherry-pick <commit-hash>

# 连续拿多个
git cherry-pick <hash-a> <hash-b> <hash-c>

# 拿一段连续的 commit（不含 hash-a 本身）
git cherry-pick <hash-a>..<hash-c>
```

**应用场景：**
- hotfix 从 main 同步到多个 release 分支
- 只想合并某几个 commit，不 merge 整个分支

---

## 7. git submodule — 子模块基础

一个仓库里引用另一个仓库。

```bash
# 添加子模块
git submodule add https://github.com/user/lib.git lib/

# 克隆带子模块的仓库
git clone --recurse-submodules <url>
# 或者已经 clone 了
git submodule update --init --recursive

# 更新子模块到最新
git submodule update --remote

# 删除子模块（步骤较多）
git submodule deinit -f lib/
git rm lib/
rm -rf .git/modules/lib/
```

> ⚠️ Submodule 容易踩坑。简单场景用包管理（npm/pip/go mod）替代更省心。

---

## 8. git alias — 自定义别名

把长命令变成短名字。

```bash
# 全局别名（~/.gitconfig）
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all --decorate"

# 用起来
git lg          # 漂亮的 commit 树
git ci -m "msg" # 等价于 git commit -m
```

**我的 alias 配置参考：**
```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.aa "add --all"
git config --global alias.ps "push"
git config --global alias.pl "pull --rebase"
git config --global alias.df "diff"
git config --global alias.dfc "diff --cached"
git config --global alias.undo "reset --soft HEAD~1"
```

**验证：**
```bash
git config --global --list | grep alias
```
