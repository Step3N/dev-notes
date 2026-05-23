# GitHub CLI (gh)

`gh` 是 GitHub 官方命令行工具，搞定 PR、Issue、Release 不用开浏览器。

---

## 安装

**macOS：**
```bash
brew install gh
```

**Windows（winget）：**
```powershell
winget install --id GitHub.cli
```

**Windows（scoop）：**
```powershell
scoop install gh
```

**Linux（apt）：**
```bash
(type -p wget >/dev/null || sudo apt install wget -y) \
&& sudo mkdir -p -m 755 /etc/apt/keyrings \
&& wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

**验证：**
```bash
gh --version
# gh version 2.x.x (...)
```

---

## 认证

```bash
# 交互式登录（浏览器）
gh auth login

# 或者用 token（适合 CI/无头环境）
gh auth login --with-token < mytoken.txt
```

**验证：**
```bash
gh auth status
# ✓ Logged in to github.com as yourname (/Users/you/.config/gh/hosts.yml)
```

---

## 仓库操作

```bash
# 创建仓库
gh repo create my-project --public --clone

# 从模板创建
gh repo create my-project --template owner/template-repo --public --clone

# Fork 仓库
gh repo fork owner/repo --clone

# 查看仓库信息
gh repo view owner/repo
```

---

## Pull Requests

```bash
# 创建 PR
gh pr create \
  --title "加了用户登录功能" \
  --body "实现了邮箱+密码登录" \
  --base main \
  --reviewer teammate1

# 列出 PR
gh pr list --limit 10

# 查看 PR 详情
gh pr view 42

# Review PR
gh pr review 42 --approve
gh pr review 42 --comment "这里需要改一下"
gh pr review 42 --request-changes

# 合并 PR
gh pr merge 42 --merge        # merge commit
gh pr merge 42 --squash       # squash merge
gh pr merge 42 --rebase       # rebase merge

# 检出 PR 到本地
gh pr checkout 42
```

---

## Issues

```bash
# 创建 issue
gh issue create \
  --title "登录页样式错乱" \
  --body "在 Chrome 下按钮位置偏移" \
  --label "bug","ui"

# 列出 issues
gh issue list --assignee @me
gh issue list --label bug

# 查看详情
gh issue view 123

# 关闭/ reopen
gh issue close 123
gh issue reopen 123
```

---

## Releases

```bash
# 列出 releases
gh release list

# 创建 release
gh release create v1.0.0 \
  --title "v1.0.0" \
  --notes "第一个正式版本" \
  dist/*.tar.gz

# 下载 release 资源
gh release download v1.0.0 -D ./downloads

# 查看 release 详情
gh release view v1.0.0
```

---

## Gist

```bash
# 创建 gist
gh gist create script.py --public -d "批量处理脚本"

# 列出自己的 gist
gh gist list --limit 20

# 查看 gist
gh gist view <gist-id>

# 编辑 gist
gh gist edit <gist-id>
```

---

## CI 中使用

GitHub Actions 里默认已认证，直接使用：

```yaml
# .github/workflows/pr-check.yml
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: 添加 PR 评论
        run: gh pr comment ${{ github.event.pull_request.number }} --body "✅ CI 通过"
        env:
          GH_TOKEN: ${{ github.token }}
```

**常用环境变量：**
| 变量 | 说明 |
|------|------|
| `GH_TOKEN` | 认证 token |
| `GH_HOST` | 指定 GitHub 实例（如 `github.company.com`） |
| `GH_REPO` | 指定仓库（格式 `owner/repo`） |

---

## 常用别名

我自己常用的：

```bash
# 在 shell 配置里加（~/.zshrc / ~/.bashrc / $PROFILE）
alias gpr="gh pr create --fill"
alias gprl="gh pr list"
alias gprv="gh pr view --web"
alias gprco="gh pr checkout"
alias ghi="gh issue create --label"
alias ghil="gh issue list"
alias ghrc="gh release create"
```

**配合 fzf 查看 PR（进阶）：**
```bash
alias gprf="gh pr list --json number,title,author,headRefName | jq -r '.[] | \"\(.number) \(.title) (\(.author.login)) [\(.headRefName)]\"' | fzf | awk '{print \$1}' | xargs gh pr view --web"
```

---

## 快速参考

```bash
gh repo create       # 创建仓库
gh repo fork         # fork 仓库
gh pr create         # 创建 PR
gh pr list           # 列出 PR
gh pr checkout       # 检出 PR
gh pr review         # review PR
gh pr merge          # 合并 PR
gh issue create      # 创建 issue
gh issue list        # 列出 issue
gh release create    # 发版
gh gist create       # 创建 gist
gh auth login        # 登录
```

> 更多：`gh help` 或 https://cli.github.com/manual/
