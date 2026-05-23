# VS Code 配置同步

全平台通用。

---

## 方法一：内置 Settings Sync（推荐）

VS Code 内置配置同步功能，支持 GitHub 和 Microsoft 账号。

### 开启同步

1. 点击左下角齿轮图标 ⚙️ → **Turn on Settings Sync…**
2. 选择登录方式：**Sign in with GitHub** 或 **Sign in with Microsoft**
3. 选择要同步的内容：
   - `Settings` — settings.json
   - `Keyboard Shortcuts` — keybindings.json
   - `Extensions` — 已安装扩展列表
   - `User Snippets` — 代码片段
   - `UI State` — 布局/视图状态

### 查看同步状态

点击齿轮图标 → **Settings Sync is On** → 查看上次同步时间与冲突。

### 手动触发同步

齿轮图标 → **Settings Sync: Sync Now** 或命令面板 → `Settings Sync: Sync Now`

### 冲突处理

当多台机器配置冲突时：
- **Merge**（合并）：尝试自动合并双方更改
- **Replace Local**（覆盖本地）：以云端为准
- **Replace Remote**（覆盖云端）：以本地为准

### 切换同步内容

齿轮图标 → **Settings Sync: Configure…** → 勾选/取消具体同步项。

### 关闭同步

齿轮图标 → **Settings Sync: Turn Off**。

### 重置同步数据

齿轮图标 → **Settings Sync: Reset Sync Data**（清除云端数据，慎用）。

---

## 方法二：Settings.json 作为 Dotfiles（手动 Git 管理）

适合希望完全掌控配置的用户。

### 配置文件路径

| 平台 | 配置目录 |
|------|---------|
| **macOS** | `~/Library/Application Support/Code/User/` |
| **Windows** | `%APPDATA%\Code\User\` |
| **Linux** | `~/.config/Code/User/` |

主要文件：

```
settings.json          — 编辑器设置
keybindings.json       — 快捷键自定义
snippets/*.code-snippets — 代码片段
```

### 使用 Git 管理

```bash
# macOS / Linux
mkdir -p ~/dotfiles/vscode
cp ~/Library/Application\ Support/Code/User/settings.json ~/dotfiles/vscode/
cp ~/Library/Application\ Support/Code/User/keybindings.json ~/dotfiles/vscode/
cp -r ~/Library/Application\ Support/Code/User/snippets ~/dotfiles/vscode/
```

```powershell
# Windows PowerShell
mkdir ~/dotfiles/vscode -Force
cp "$env:APPDATA\Code\User\settings.json" ~/dotfiles/vscode/
cp "$env:APPDATA\Code\User\keybindings.json" ~/dotfiles/vscode/
cp -r "$env:APPDATA\Code\User\snippets" ~/dotfiles/vscode/
```

### 在新机器上恢复

```bash
# 创建软链接
# macOS / Linux
ln -sf ~/dotfiles/vscode/settings.json ~/Library/Application\ Support/Code/User/settings.json
ln -sf ~/dotfiles/vscode/keybindings.json ~/Library/Application\ Support/Code/User/keybindings.json
```

```powershell
# Windows PowerShell（需管理员权限）
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\Code\User\settings.json" -Target ~/dotfiles/vscode/settings.json
New-Item -ItemType SymbolicLink -Path "$env:APPDATA\Code\User\keybindings.json" -Target ~/dotfiles/vscode/keybindings.json
```

### 同步扩展

```bash
# 导出
code --list-extensions > ~/dotfiles/vscode/extensions.txt

# 导入
cat ~/dotfiles/vscode/extensions.txt | xargs -L1 code --install-extension
```

---

## 对比总结

| 特性 | 内置 Settings Sync | 手动 Dotfiles |
|------|-------------------|---------------|
| 易用性 | ⭐⭐⭐⭐⭐ 一键开启 | ⭐⭐ 需手动配置 |
| 扩展同步 | 自动同步 ID | 需手动导出/导入 |
| 版本控制 | 无（仅有云端快照） | Git 完整历史 |
| 多账号 | 支持 | 不涉及 |
| 冲突处理 | 内置合并 UI | 需手动 Git Merge |

**推荐**：大多数人使用内置 Settings Sync 即可。追求极致的用户可额外用 Dotfiles 方式管理 `settings.json` 并用 Git 追踪变更。
