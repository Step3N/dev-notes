# Cursor IDE 笔记

Cursor 是基于 VS Code 的 AI-native IDE，AI 集成比 Copilot 更深入。下载：https://cursor.com

---

## 从 VS Code 迁移

Cursor 直接兼容 VS Code 生态：

- **扩展**：直接安装 VS Code 扩展（`cursor` 命令行在设置中配置）
- **设置**：`Cmd+Shift+P` → `Preferences: Open Settings`，支持同步 VS Code 配置
- **快捷键**：预装 VS Code 快捷键方案，可切换至 Vim/Emacs
- **主题/图标**：直接用 VS Code 市场的主题

```bash
# 安装 cursor CLI 工具（在 Cursor 设置里开启）
# 然后就可以在终端用 cursor 命令打开文件
cursor .
cursor src/app.ts
```

---

## 关键功能

### Ctrl+K / Cmd+K：AI 编辑

选中一段代码，`Cmd+K` 弹出编辑框，描述你要的改动，AI 直接修改代码：

1. 选中代码片段
2. 按 `Cmd+K`
3. 输入修改描述（如"改成分页加载"）
4. 确认或拒绝修改

### Ctrl+L / Cmd+L：AI 对话

基于整个代码库的上下文对话：
- 自动包含当前文件和选择
- 可手动添加上下文

**上下文选择器**（在对话中输入 `@` 触发）：

| 指令 | 作用 |
|------|------|
| `@file` | 引用特定文件 |
| `@folder` | 引用整个目录 |
| `@codebase` | 搜索整个代码库 |
| `@web` | 联网搜索 |
| `@docs` | 引用文档 |
| `@git` | 引用 git 变更 |

### Composer (Ctrl+I)

多文件编辑模式，适合：
- 创建新组件（同时生成 .tsx, .css, test 文件）
- 跨文件重构
- 从零开始生成项目

```text
# 在 Composer 中输入：
创建一个 TodoList React 组件，
包含 TodoList.tsx、TodoList.test.tsx、TodoList.css
```

Composer 会一次生成多个文件。

### Tab 补全

Cursor 的 Tab 补全比 Copilot 更进一步：
- 不仅补全代码行，还能预测下一步操作
- 灰色"cursor prediction"提示你下一个编辑位置
- Tab 接受后自动跳转到下一个合理位置

### Apply vs Diff

AI 修改代码时有两种应用模式：
- **Apply**：直接替换文件内容
- **Diff**：显示 diff 后再确认，更安全

---

## 模型支持

| 模型 | 来源 | 说明 |
|------|------|------|
| GPT-4o | OpenAI | 默认，综合能力好 |
| Claude 3.5 Sonnet | Anthropic | 代码能力极强 |
| Claude 4 Sonnet | Anthropic | 最新 |
| o3/o4-mini | OpenAI | 推理任务 |
| Custom | 自己的 API Key | 可在设置中配置 |

> Pro 订阅 ($20/月) 有更多使用额度。也可绑定自己的 API Key 在 Cursor 里用。

---

## Cursor Rules

在项目根目录创建 `.cursorrules` 文件，定制 AI 行为：

```
你是一个 React + TypeScript 前端工程师。
- 使用函数式组件 + Hooks
- 样式用 Tailwind CSS
- 测试用 Vitest
- 避免使用 any 类型
- API 调用使用 react-query
- 文件命名：PascalCase 对于组件，camelCase 对于工具函数
```

更好的方案：`.cursor/rules/` 目录下放多条规则文件。

---

## VS Code + Copilot vs Cursor

| 维度 | VS Code + Copilot | Cursor |
|------|------------------|--------|
| 代码补全 | 优秀 | 优秀 + 预测编辑 |
| 内联编辑 | 有 (Ctrl+I) | 更深集成 (Cmd+K) |
| 多文件编辑 | 无 | Composer |
| 上下文理解 | 当前文件/工作区 | 更精准的上下文选择 |
| 扩展生态 | 完整 VS Code | 完整 VS Code |
| 价格 | Copilot $10 + VS Code 免费 | Pro $20/月 |
| 学习成本 | 低 | 中等 |

**选哪个？** 如果用 Copilot 已经够用就继续用。Cursor 适合需要更深度 AI 集成的场景，尤其是"agentic"编程（让 AI 理解项目整体帮你写代码）。

---

## 平台

全平台支持：macOS / Windows / Linux。
