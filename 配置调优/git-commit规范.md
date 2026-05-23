# Git Commit 规范 — Conventional Commits

Commit message 写得好不好，直接决定 `git log` 能不能看、changelog 能不能自动生成。

---

## 格式

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**真实例子：**
```
feat(login): add email/password authentication

Implemented JWT-based login flow with refresh token rotation.
Closes #42
```

**每部分说明：**

| 部分 | 必须？ | 说明 |
|------|--------|------|
| `type` | ✅ | 提交类型 |
| `scope` | ❌ | 影响范围（模块名） |
| `description` | ✅ | 简短描述（小写开头，不加句号） |
| `body` | ❌ | 详细说明，和标题空一行 |
| `footer` | ❌ | 关联 issue、breaking changes |

---

## 常用 Types

| Type | 含义 | 版本影响 |
|------|------|----------|
| `feat` | 新功能 | 次版本+1 |
| `fix` | Bug 修复 | 补丁版本+1 |
| `docs` | 文档修改 | 无 |
| `refactor` | 重构（不改功能不改 bug） | 无 |
| `style` | 代码格式（空格、分号等） | 无 |
| `test` | 加测试 | 无 |
| `chore` | 构建/工具链/依赖 | 无 |
| `perf` | 性能优化 | 无 |
| `ci` | CI 配置变更 | 无 |
| `build` | 构建系统变更 | 无 |
| `revert` | 回退提交 | 看情况 |

**breaking change 标记：**
```
feat(auth): switch from JWT to session-based auth

BREAKING CHANGE: removed all JWT endpoints, old tokens will not work
```
或者加 `!`：
```
feat(auth)!: switch from JWT to session-based auth
```

---

## 好 vs 坏

### 😊 好的 commit

```
feat(api): add user search endpoint
fix(api): handle null response in user search
docs(readme): update installation steps
refactor(db): extract query builder to separate module
test(auth): add unit tests for token refresh
chore(deps): upgrade axios to 1.7.0
```

### 😠 差的 commit

```
fix stuff                              # 改了什么？
update                                 # 更新什么了？
asdfasdf                               # 随便打的
wip                                    # 谁记得这是啥
fixed bug                              # 什么 bug？
merge branch 'main' into feature/login # 这是 merge 信息，不是 commit
```

---

## 为什么要规范？

1. **自动生成 CHANGELOG**
   ```bash
   # conventional-changelog 自动生成
   npx conventional-changelog -p angular -i CHANGELOG.md -s
   ```

2. **自动语义化版本**
   ```
   feat → minor version bump
   fix  → patch version bump
   BREAKING CHANGE → major version bump
   ```

3. **git log 可读性暴增**
   ```bash
   # 只看某个类型的提交
   git log --oneline --grep="^feat"

   # 只看某个范围的改动
   git log --oneline --grep="^fix(api)"
   ```

---

## 工具：Commitizen (cz-cli)

交互式生成符合规范的 commit：

```bash
# 全局安装
npm install -g commitizen

# 初始化适配器（项目级）
npx commitizen init cz-conventional-changelog --save-dev

# 替代 git commit
git cz
```

**vscode 用户**：装插件 **Conventional Commits**，一键生成标准 commit。

---

## 工具：commitlint

提交时自动校验 message 格式。

```bash
# 安装
npm install -D @commitlint/{config-conventional,cli}

# 配置
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

配合 husky 在 commit-msg hook 里校验：

```bash
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'
```

---

## 团队规范建议

1. **统一 type 列表** — 上面列的那些够用，别自己发明
2. **description 用英文或中文都行**，但团队统一
3. **scope 视项目大小决定** — 小项目不用 scope，大项目用它按模块分组
4. **关联 issue** — fix 或 feat 在 footer 写 `Closes #123`
5. **不要一条 commit 改一堆东西** — 一个 commit 只做一件事

### 最佳实践示例

```bash
# feature 分支的开发流程
git commit -m "feat(settings): add notification preferences page"
git commit -m "test(settings): add tests for notification preferences"
git commit -m "docs(settings): add API docs for notification endpoints"
```

```bash
# bug 修复
git commit -m "fix(editor): fix crash when saving empty document"
git commit -m "test(editor): add regression test for empty save"
```
