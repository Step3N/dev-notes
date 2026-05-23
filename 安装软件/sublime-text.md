# Sublime Text

轻量、极速、专注。打开即用，启动速度毫秒级，处理几百 MB 的大文件也比其他编辑器流畅得多。

---

## 安装

**macOS**

```bash
brew install --cask sublime-text
```

**Windows**

```powershell
winget install SublimeHQ.SublimeText.4
```

**Linux**

```bash
# 从官网下载 .deb / .rpm / .tar.xz
# https://www.sublimetext.com/download
```

验证：

```bash
subl --version
```

---

## 为什么用 Sublime Text？

| 场景 | 优势 |
|------|------|
| 超大文件（GB 级日志） | 几乎不卡顿，VS Code/IDEA 可能直接崩溃 |
| 启动速度 | 秒开，无需等待索引 |
| 专注写作 | 全屏 + 免打扰模式（`⇧⌘CtrlF`） |
| 远程编辑 | 配合 SFTP 插件，直接编辑服务器文件 |
| 低配机器 | 内存占用量远低于 Electron 编辑器 |

---

## Package Control — 插件管理

Sublime Text 的包管理器，也是第一个要装的插件。

```txt
打开 Sublime Text → `⌘⇧P` / `Ctrl+Shift+P` → 输入 Install Package Control → 回车
```

安装完毕后，`⌘⇧P` → `Package Control: Install Package` → 搜索插件名称。

---

## 必装插件

| 插件 | 说明 |
|------|------|
| **A File Icon** | 侧边栏文件图标增强 |
| **BracketHighlighter** | 括号匹配高亮 |
| **SublimeLinter** | 代码 lint（需配对应语言的 linter） |
| **Git Gutter** | 行内 Git diff（增删改标记） |
| **Emmet** | HTML/CSS 快速编写（`! + Tab` 生成模板） |
| **Pretty JSON** | JSON 格式化 |
| **SFTP** | 远程文件编辑 |
| **SideBarEnhancements** | 侧边栏右键菜单增强 |

安装方法：`⌘⇧P` → `Package Control: Install Package` → 输入插件名 → 回车。

---

## 关键快捷键

| 操作 | macOS | Windows / Linux |
|------|-------|-----------------|
| Goto Anything | `⌘P` | `Ctrl+P` |
| 命令面板 | `⌘⇧P` | `Ctrl+Shift+P` |
| 搜索当前文件 | `⌘F` | `Ctrl+F` |
| 全局搜索 | `⌘⇧F` | `Ctrl+Shift+F` |
| 分屏 | `⌘⌥2` | `Ctrl+Alt+2` |
| 侧边栏开关 | `⌘K, ⌘B` | `Ctrl+K, Ctrl+B` |
| 免打扰模式 | `⇧⌘CtrlF` | `Shift+Ctrl+F` |
| 跳转到行 | `⌘P` → 输入 `:行号` | `Ctrl+P` → `:行号` |

### 多光标特性

Sublime Text 的多光标是最强大的之一：

| 操作 | macOS | Windows / Linux |
|------|-------|-----------------|
| 选同词 | `⌘D` | `Ctrl+D` |
| 跳过当前 | `⌘K, ⌘D` | `Ctrl+K, Ctrl+D` |
| 列选择 | `⌥ + 拖拽` | `Alt + 拖拽` |
| 全选同词 | `⌘⇧L` | `Ctrl+Shift+L` |

---

## 自定义配置

打开方式：`⌘⇧P` → `Preferences: Settings`。

这是 `Preferences.sublime-settings` 推荐配置：

```json
{
  "font_size": 14,
  "font_face": "JetBrains Mono",
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "rulers": [100],
  "highlight_line": true,
  "bold_folder_labels": true,
  "auto_save": true,
  "wide_caret": true,
  "ensure_newline_at_eof_on_save": true,
  "trim_trailing_white_space_on_save": true,
  "word_wrap": false
}
```

---

## 命令行启动

安装时若未自动加入 PATH，手动添加：

**macOS**

```bash
ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl
```

**Windows** — 安装时默认勾选 "Add to PATH"，确认：

```powershell
subl --version
```

用法：

```bash
subl .                    # 打开当前文件夹为项目
subl file.txt             # 打开文件
subl --new-window .       # 新建窗口打开项目
```

---

## 平台差异

| 项目 | macOS | Windows | Linux |
|------|-------|---------|-------|
| 配置目录 | `~/Library/Application Support/Sublime Text/Packages/User/` | `%APPDATA%\Sublime Text\Packages\User\` | `~/.config/sublime-text/Packages/User/` |
| 设置文件 | `Preferences.sublime-settings` | 同 | 同 |
| 快捷键 | `Default (OSX).sublime-keymap` | `Default (Windows).sublime-keymap` | `Default (Linux).sublime-keymap` |

---

## 🔗 参考

- 官网: https://www.sublimetext.com
- Package Control: https://packagecontrol.io
- 快捷键参考: https://sublimetext.com/docs/keyboard.html
