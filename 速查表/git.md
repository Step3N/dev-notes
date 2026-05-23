# Git 速查表

> 全平台通用 · 常用命令速查

## 配置 Setup & Config

| 命令 | 说明 |
|------|------|
| `git config --global user.name "name"` | 设置全局用户名 |
| `git config --global user.email "email"` | 设置全局邮箱 |
| `git config --global core.editor vim` | 设置默认编辑器 |
| `git config --global alias.xxx <cmd>` | 设置别名，如 `co` → `checkout` |
| `git config --list` | 查看当前配置 |
| `git config --global core.autocrlf input` | macOS/Linux 换行符处理 |
| `git config --global init.defaultBranch main` | 默认分支名 |

## 日常工作 Daily Workflow

| 命令 | 说明 |
|------|------|
| `git init` | 初始化仓库 |
| `git clone <url>` | 克隆远程仓库 |
| `git clone --depth=1 <url>` | 浅克隆（只取最新提交） |
| `git add <file>` | 暂存文件 |
| `git add -A` / `git add .` | 暂存所有变更 |
| `git add -p` | 交互式分块暂存 |
| `git commit -m "msg"` | 提交暂存区 |
| `git commit -am "msg"` | 暂存已跟踪文件 + 提交（跳过 add） |
| `git commit --amend -m "msg"` | 修改上次提交信息 |
| `git log --oneline --graph --all` | 简洁提交历史图 |
| `git log -p` | 查看提交 diff |

## 分支与合并 Branch & Merge

| 命令 | 说明 |
|------|------|
| `git branch` | 列出本地分支（`*` 为当前） |
| `git branch -a` | 列出所有分支（含远程） |
| `git branch <name>` | 创建分支 |
| `git branch -d <name>` | 删除分支（已合并） |
| `git branch -D <name>` | 强制删除分支 |
| `git switch <name>` | 切换分支（新版） |
| `git switch -c <name>` | 创建并切换 |
| `git checkout -b <name>` | 创建并切换（旧版） |
| `git merge <name>` | 合并分支到当前 |
| `git merge --no-ff <name>` | 禁止快进合并 |
| `git rebase <name>` | 变基到目标分支 |
| `git rebase -i HEAD~n` | 交互式变基（合并/修改 n 个提交） |

## 检查与比较 Inspect & Compare

| 命令 | 说明 |
|------|------|
| `git status` | 查看工作区状态 |
| `git diff` | 工作区 vs 暂存区差异 |
| `git diff --staged` | 暂存区 vs 上次提交差异 |
| `git diff <a>..<b>` | 两个分支/提交差异 |
| `git show <commit>` | 查看某次提交详情 |
| `git blame <file>` | 逐行查看文件作者 |
| `git log <file>` | 查看文件提交历史 |

## 撤销 Undo

| 命令 | 说明 |
|------|------|
| `git restore <file>` | 丢弃工作区修改 |
| `git restore --staged <file>` | 取消暂存（→工作区） |
| `git reset --soft HEAD~1` | 撤销提交，改动留在暂存区 |
| `git reset --mixed HEAD~1` | 撤销提交，改动回工作区 |
| `git reset --hard HEAD~1` | 撤销提交，丢弃所有改动 |
| `git revert <commit>` | 通过新提交回滚某次提交 |

## Stash 暂存

| 命令 | 说明 |
|------|------|
| `git stash` | 暂存未提交的改动 |
| `git stash push -m "msg"` | 暂存并加备注 |
| `git stash list` | 列出所有 stash |
| `git stash pop` | 恢复最近 stash 并删除 |
| `git stash apply stash@{n}` | 恢复指定 stash（不删除） |
| `git stash drop stash@{n}` | 删除指定 stash |
| `git stash clear` | 清除所有 stash |

## 远程操作 Remote

| 命令 | 说明 |
|------|------|
| `git remote -v` | 查看远程仓库地址 |
| `git remote add origin <url>` | 添加远程仓库 |
| `git remote rm <name>` | 删除远程仓库 |
| `git fetch` | 拉取远程数据（不合并） |
| `git pull` | `fetch` + `merge` |
| `git pull --rebase` | `fetch` + `rebase` |
| `git push` | 推送到远程 |
| `git push -u origin <branch>` | 推送并建立跟踪关系 |
| `git push origin --delete <branch>` | 删除远程分支 |
| `git push --tags` | 推送所有标签 |
