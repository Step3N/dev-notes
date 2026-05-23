# Codeium — 免费 AI 编程助手

Codeium 是个人开发者免费的 AI 编程助手，支持最多 IDE 平台。

---

## 核心信息

| 项目 | 说明 |
|------|------|
| 价格 | 个人免费，团队收费 |
| 网站 | https://codeium.com |
| 模型 | 自有模型 |
| 主要功能 | 代码补全 + Chat + 语义搜索 |

---

## 安装

### VS Code

扩展市场搜索 `Codeium`，安装后右下角弹出登录页面。

### JetBrains

Preferences → Plugins → 搜索 `Codeium`，安装后重启。

### 其他 IDE

支持列表：Neovim, Vim, Emacs, Eclipse, Sublime Text, Xcode, Android Studio。

```bash
# Neovim 安装
{
  "Exafunction/codeium.nvim",
  config = function()
    require("codeium").setup({})
  end
}
```

---

## 功能

### 代码补全

自动触发，与 Copilot 类似。Tab 接受，Esc 拒绝。

```python
def fibonacci(n):
    # 输入 def f 后 Codeium 自动建议
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### Chat

侧边栏对话，支持 `@file` 引用代码上下文。质量不如 Copilot Chat 和 Cursor。

### 语义搜索 (Codeium Search)

Codeium 的特色功能，基于语义理解搜索代码库：

```
# 不需要记得函数名
搜索："把用户信息保存到数据库的代码在哪"
# 而不是搜："save_user_to_db"
```

可在侧边栏 Search 面板使用，也支持自然语言搜索。

---

## 与 Copilot 对比

| 维度 | Codeium | Copilot |
|------|---------|---------|
| 个人价格 | 免费 | $10/月 |
| 团队价格 | 付费 | $19/月 |
| IDE 支持 | 最多（10+ IDE） | VS Code, JetBrains, Neovim |
| 补全质量 | 良好，略逊于 Copilot | 优秀 |
| Chat 质量 | 中等 | 好 |
| 语义搜索 | 有 | 无 |
| 中文支持 | 一般 | 一般 |

**Codeium 的优势**：免费、IDE 覆盖广、有语义搜索。

**Codeium 的劣势**：复杂任务补全质量不如 Copilot、Chat 能力较弱。

---

## Windsurf IDE

Codeium 团队推出的 AI-native IDE，类似 Cursor。

下载: https://codeium.com/windsurf

特点：
- 基于 VS Code 的独立 IDE
- 更深度的 AI 集成（类似 Cursor 的 Composer）
- 免费版有每日使用限制
- Pro 版 $15/月

---

## 使用建议

- **个人开发者**：免费首选，够用
- **全栈/复杂项目**：Copilot 或 Cursor 更好
- **多 IDE 用户**：Codeium 覆盖最广
- **需要语义搜索**：Codeium 独有功能

---

## 平台

全平台支持。安装简便，注册即可使用。
