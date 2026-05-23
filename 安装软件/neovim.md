# Neovim — 现代化 Vim

Neovim 是 Vim 的一个分支，核心目标是**更好的可扩展性**。最大的区别：原生 Lua 支持、异步插件、内置 LSP 生态。

---

## 安装

**macOS**

```bash
brew install neovim
```

**Windows**

```powershell
winget install Neovim.Neovim
# 或 Scoop
scoop install neovim
```

**Linux**

```bash
# Debian / Ubuntu
sudo apt install neovim

# Fedora
sudo dnf install neovim

# Arch
sudo pacman -S neovim
```

验证：

```bash
nvim --version
```

---

## 配置文件位置

| 平台 | 路径 |
|------|------|
| **macOS** / **Linux** | `~/.config/nvim/` |
| **Windows** | `~/AppData/Local/nvim/` |

主配置文件是 `init.lua`（Neovim 推荐 Lua，不再用 `.vimrc`）。

---

## 基础配置 (`~/.config/nvim/init.lua`)

```lua
-- 基础选项
vim.opt.number = true         -- 显示行号
vim.opt.relativenumber = true -- 相对行号
vim.opt.tabstop = 2
vim.opt.shiftwidth = 2
vim.opt.expandtab = true
vim.opt.mouse = "a"           -- 启用鼠标
vim.opt.clipboard = "unnamedplus" -- 系统剪贴板

-- 快捷键映射
vim.g.mapleader = " "         -- leader 键设为空格
vim.keymap.set("n", "<leader>w", ":w<CR>", { desc = "保存文件" })
vim.keymap.set("n", "<leader>q", ":q<CR>", { desc = "退出" })
```

---

## 插件管理器：lazy.nvim

lazy.nvim 是目前 Neovim 最流行的插件管理器：

```lua
-- ~/.config/nvim/init.lua

local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", lazypath
  })
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup({
  -- 插件写在这里
})
```

---

## 必备插件

### telescope.nvim — 模糊查找

文件、grep、帮助文档等一切内容的模糊搜索入口：

```lua
require("lazy").setup({
  {
    "nvim-telescope/telescope.nvim",
    dependencies = { "nvim-lua/plenary.nvim" },
    config = function()
      local builtin = require("telescope.builtin")
      vim.keymap.set("n", "<leader>ff", builtin.find_files, { desc = "查找文件" })
      vim.keymap.set("n", "<leader>fg", builtin.live_grep, { desc = "内容搜索" })
      vim.keymap.set("n", "<leader>fb", builtin.buffers, { desc = "缓冲区列表" })
    end
  },
})
```

### nvim-treesitter — 语法高亮

比传统正则高亮更准确、更快：

```lua
{
  "nvim-treesitter/nvim-treesitter",
  build = ":TSUpdate",
  config = function()
    require("nvim-treesitter.configs").setup({
      ensure_installed = { "lua", "python", "rust", "go", "javascript", "typescript" },
      highlight = { enable = true },
    })
  end,
}
```

### nvim-lspconfig + mason.nvim — LSP 集成

一键安装语言服务器，自动配置 LSP 客户端：

```lua
{
  "williamboman/mason.nvim",
  build = ":Mason",
  config = true,
}

{
  "williamboman/mason-lspconfig.nvim",
  dependencies = { "mason.nvim" },
  opts = {
    ensure_installed = { "lua_ls", "pyright", "rust_analyzer", "gopls", "ts_ls" },
  },
}

{
  "neovim/nvim-lspconfig",
  dependencies = { "mason-lspconfig.nvim" },
  config = function()
    local lspconfig = require("lspconfig")
    -- 所有由 mason 安装的 LSP 自动配置
    require("mason-lspconfig").setup_handlers({
      function(server_name)
        lspconfig[server_name].setup({})
      end,
    })
    -- 快捷键
    vim.keymap.set("n", "gd", vim.lsp.buf.definition, { desc = "跳转到定义" })
    vim.keymap.set("n", "K", vim.lsp.buf.hover, { desc = "悬停文档" })
    vim.keymap.set("n", "<leader>ca", vim.lsp.buf.code_action, { desc = "代码操作" })
  end,
}
```

---

## Neovim vs Vim：核心差异

| 特性 | Vim | Neovim |
|------|-----|--------|
| 配置语言 | Vimscript | Lua（推荐，也可用 Vimscript） |
| 异步任务 | 有限 | ✅ 原生支持 |
| 内置 LSP | ❌ | ✅ `vim.lsp.*` API |
| 插件生态 | 传统 Vim 插件 | 可利用全部 Vim 插件 + Lua 插件 |
| 终端模拟 | 有限 | ✅ `:terminal` 完整终端 |
| GUI 框架 | gVim | 可独立前端（如 Neovide） |

---

## 验证 LSP 是否工作

打开任意代码文件：

```bash
nvim main.go
# 在 Normal 模式下按 gd 应该能跳转到定义
# 按 K 应该能看到文档提示
```

---

## 🔗 参考

- 官网: https://neovim.io
- lazy.nvim: https://github.com/folke/lazy.nvim
- Neovim 中文文档: https://github.com/neovim/neovim
