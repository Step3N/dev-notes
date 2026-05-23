# AI 编程助手对比

主流 AI 编程助手的横向对比，帮你选最合适的。

---

## 功能对比

| 特性 | GitHub Copilot | Cursor IDE | Codeium | 通义灵码 |
|------|---------------|-----------|---------|---------|
| 价格 | $10/月 (个人) | $20/月 (Pro) | 免费 (个人) | 免费 |
| 代码补全 | ★★★★★ | ★★★★ | ★★★★ | ★★★ |
| Chat 对话 | ★★★★ | ★★★★★ | ★★★ | ★★★ |
| 多文件编辑 | ✗ | ✓ (Composer) | ✗ | ✗ |
| 内联代码编辑 | ✓ | ★★★★★ | ✓ | ✓ |
| 语义搜索 | ✗ | ✗ | ✓ | ✗ |
| Agent 模式 | ✓ (最新) | ✓ | ✗ | ✗ |
| 上下文选择器 | @workspace | @file @folder @codebase @web | @file | @workspace |
| 项目定制 | 无 | .cursorrules | 无 | 无 |

---

## IDE 支持

| 助手 | VS Code | JetBrains | Neovim | Eclipse | Xcode | 其他 |
|------|---------|-----------|--------|---------|-------|------|
| Copilot | ✓ | ✓ | ✓ | ✗ | ✓ | Visual Studio |
| Cursor | 自身 IDE | ✗ | ✗ | ✗ | ✗ | VS Code fork |
| Codeium | ✓ | ✓ | ✓ | ✓ | ✓ | Sublime, Emacs |
| 通义灵码 | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |

---

## 语言/中文支持

| 助手 | 中文注释理解 | 中文对话 | 多语言泛化 |
|------|------------|---------|-----------|
| Copilot | 一般 | 一般 | ★★★★★ |
| Cursor | 一般 | 好（模型决定） | ★★★★★ |
| Codeium | 一般 | 一般 | ★★★★ |
| 通义灵码 | ★★★★★ | ★★★★★ | ★★★ |

---

## 场景推荐

### 追求最佳补全体验 → GitHub Copilot

Copilot 的代码补全准确度仍然最高。如果你主要用 VS Code 或 JetBrains，愿意付 $10/月，Copilot 是最稳妥的选择。

### 追求 AI-native 开发 → Cursor IDE

Cursor 的 Composer、Cmd+K 内联编辑、.cursorrules 定制深度远超 Copilot。适合愿意尝试新工具、追求"让 AI 写更多代码"的开发者。价格 $20/月。

### 需要免费方案 → Codeium

Codeium 个人版完全免费，支持最多 IDE，还有独特的语义搜索。补全质量虽然略逊 Copilot，但性价比无敌。适合学生、个人项目或预算有限的团队。

### 中文生态优先 → 通义灵码

如果你用中文写注释、需要中文对话、或者主力语言是 Java，通义灵码值得一试。完全免费，阿里系生态集成好。

### 混合方案

不冲突，可以同时用：

```
# VS Code 里：
- Codeium 做免费补全
- 聊天用 ChatGPT / Claude 网页版

# Cursor 里：
- 自带 AI 功能
- 用 .cursorrules 定制行为

# 需要写中文注释的项目：
- 主力用 Cursor 或 Copilot
- 结合通义灵码处理中文相关任务
```

---

## 关键决策因素

```
你需要什么？
├── 补全质量最重要 → GitHub Copilot
├── AI 深度编辑 → Cursor IDE
├── 免费 → Codeium
├── 中文/Java → 通义灵码
└── 不确定 → 先用免费的试
```

---

## 平台

所有工具均支持全平台（macOS / Windows / Linux），但 IDE 兼容性不同，详见上表。
