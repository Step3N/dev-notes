# GitHub Copilot 笔记

GitHub Copilot 是目前最成熟的 AI 编程助手，基于 OpenAI 模型，深度集成在主流 IDE 中。

---

## 订阅方案

| 方案 | 价格 | 适用 |
|------|------|------|
| Individual | $10/月 (或 $100/年) | 个人开发者 |
| Business | $19/用户/月 | 团队 |
| Enterprise | $39/用户/月 | 企业，含合规功能 |

> 学生和开源维护者可免费申请：https://education.github.com/

---

## 安装

### VS Code

1. 打开扩展面板，搜索 `GitHub Copilot`
2. 安装 GitHub Copilot 和 GitHub Copilot Chat 两个扩展
3. 点击状态栏 Copilot 图标登录 GitHub 账号

### JetBrains

1. Preferences → Plugins → 搜索 `GitHub Copilot`
2. 安装后重启 IDE
3. Tools → GitHub Copilot → Login to GitHub

### Neovim

```bash
# 使用插件管理器，如 lazy.nvim
{
  "github/copilot.vim",
  config = function()
    vim.g.copilot_enabled = 1
  end
}
```

---

## 使用方式

### 代码补全

自动触发：输入代码时 Copilot 自动给出灰色建议。
- **Tab**：接受建议
- **Alt+]**（macOS Option+]）：下一个建议
- **Alt+[**（macOS Option+[）：上一个建议
- **Esc**：取消

手动触发：`Alt+\`（macOS Option+\）强制请求建议。

### Inline Chat (Ctrl+I / Cmd+I)

光标所在行触发内联对话，直接提问让 AI 修改代码。

### Chat Panel (Ctrl+Shift+I / Cmd+Shift+I)

侧边栏聊天面板，支持：
- `@workspace`：基于整个项目上下文提问
- `@file`：指定文件上下文
- `@terminal`：包含终端输出

### Agent Mode (最新功能)

在 Chat Panel 中选择 Agent 模式，Copilot 可以：
- 编辑多个文件
- 运行终端命令
- 创建新文件
- 修复编译错误

需要 VS Code Insiders 或最新稳定版。

### Copilot Workspace (Preview)

浏览器中的功能规划工具，适合大范围重构或新功能开发。在 github.com 上访问。

---

## 有效使用技巧

### 写好注释

```javascript
// 写注释描述意图，Copilot 补全准确率会高很多

// 不好的写法
function process(data) {  // Copilot 可能猜不到你要做什么

// 好的写法
// 对用户列表按注册时间降序排序，取前 10 个
function process(data) {
```

### 提供上下文

- 打开相关文件让 Copilot 理解项目结构
- 保持函数签名完整
- 使用有意义的命名

### 适合 Copilot 的场景

- 样板代码（CRUD、配置、模板）
- 单元测试
- 正则表达式
- 配置文件（Dockerfile, YAML, JSON）
- 文档注释
- 数据转换

### 不适合 Copilot 的场景

- 全新的领域逻辑
- 安全敏感代码
- 需要深度业务理解的算法

---

## 常用快捷键

| 操作 | VS Code | JetBrains |
|------|---------|-----------|
| 接受建议 | Tab | Tab |
| 拒绝建议 | Esc | Esc |
| 下一个建议 | Alt+] | Alt+] |
| 上一个建议 | Alt+[ | Alt+[ |
| 内联对话 | Ctrl+I | Ctrl+Shift+I |
| 聊天面板 | Ctrl+Shift+I | Ctrl+Shift+I |

---

## 平台

全平台支持：VS Code, JetBrains, Neovim, Visual Studio, Xcode (通过 Copilot for Xcode)。
