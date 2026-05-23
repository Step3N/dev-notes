# 贡献指南

> 欢迎贡献！你的每一条补充、修正或新笔记都能让这个仓库更有价值。

## 贡献方式

### 🐛 报告问题
- 发现内容错误（命令失效、平台差异标注不对、错别字）
- 找到过时的配置方法
- 建议新增主题

提交 [Issue](https://github.com/Step3N/dev-notes/issues/new) 即可。

### ✏️ 提交笔记/修改

1. Fork 本仓库
2. 创建你的分支
   ```bash
   git checkout -b fix/typo-in-git-install
   # 或
   git checkout -b feature/add-ssh-tutorial
   ```
3. 修改或新增文件
4. 提交更改
   ```bash
   git commit -m "fix: correct typo in git-安装.md"
   ```
5. 推送到你的仓库
   ```bash
   git push origin fix/typo-in-git-install
   ```
6. 提交 Pull Request

### 📝 内容规范

请遵循已有笔记的格式：

#### 文件命名
- 小写字母 + 连字符：`git-日常命令.md`
- 中文名用全拼或惯用词：`虚拟环境.md`
- 速查表统一 `xxx.md`（如 `git.md`、`docker.md`）

#### 笔记结构
每篇笔记包含以下内容（可灵活调整）：

- **标题**：`# [标题]`
- **一句话说明**：`> [描述]`
- **适用平台 + 前置条件**
- **安装/获取**
- **基础配置**
- **常用命令/操作**
- **使用技巧**（可选）
- **常见问题**
- **相关资源**（可选）
- **更新记录**

#### 格式要求
| 规范 | 说明 |
|------|------|
| 代码块 | 必须标注语言：bash、powershell、python、json |
| 路径 | 反引号包裹：`~/.gitconfig` |
| 平台差异 | 用 **macOS** / **Windows** / **Linux** 标注 |
| 可验证 | 每个步骤必须有验证成功的命令 |
| 独立性 | 每篇笔记可单独阅读 |

## 代码规范

- 不要在笔记中包含真正的 API Key
- 敏感信息用 `YOUR_KEY` 或 `your-username` 代替
- 命令必须经过验证，能直接复制执行

## PR 审阅流程

提交 PR 后，维护者会尽快审阅。可能提出的修改建议：
- 补充平台差异（比如 Windows 的特殊之处）
- 添加验证命令
- 修正命令语法

## 许可证

贡献的内容将采用 MIT 许可证。

---

**Happy Contributing!** 🚀
