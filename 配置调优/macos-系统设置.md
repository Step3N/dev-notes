# macOS — 开发者系统设置推荐

开发机到手后的第一件事：把系统调教成顺手的工具，而不是和默认设置较劲。

---

## 🪄 显示隐藏文件

```bash
# 显示隐藏文件（按需切换）
defaults write com.apple.finder AppleShowAllFiles -bool true

# 显示路径栏
defaults write com.apple.finder ShowPathbar -bool true

# 显示状态栏
defaults write com.apple.finder ShowStatusBar -bool true

# 生效
killall Finder
```

> 恢复隐藏：把 `true` 改成 `false` 再 `killall Finder`。

---

## 🖥️ Finder 细节

```bash
# 设置默认展开方式为列视图
defaults write com.apple.finder FXPreferredViewStyle -string "clmv"

# 设置搜索范围默认为当前文件夹
defaults write com.apple.finder FXDefaultSearchScope -string "SCcf"

# 禁止创建 .DS_Store 文件（网络卷）
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true

killall Finder
```

---

## 🚀 开发者模式 & Xcode CLI

macOS 没有显式的"开发者模式"开关，但以下三步是关键：

```bash
# 1. 安装 Xcode Command Line Tools
xcode-select --install

# 2. 验证
xcrun --show-sdk-path
# 输出应类似: /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk

# 3. 接受许可证（如已装 Xcode）
sudo xcodebuild -license accept
```

---

## ⌨️ 键盘速度调优

```bash
# 加快按键重复速度
defaults write -g KeyRepeat -int 2           # 正常最小 2（约 30ms）
defaults write -g InitialKeyRepeat -int 15   # 重复前延迟（越小越快）

# 禁用长按弹出选字窗（让长按正常重复）
defaults write -g ApplePressAndHoldEnabled -bool false

# 生效范围：重启应用后
```

---

## 🔄 触控板 — 关闭自然滚动

```bash
# 关闭自然滚动（鼠标滚轮方向）
defaults write -g com.apple.swipescrolldirection -bool false

# 如果只想关鼠标的自然滚动，保留触控板，需分别设置：
# 在 系统设置 → 鼠标 → 自然滚动 手动关掉
```

---

## 📸 截图设置

```bash
# 修改截图保存路径（推荐放桌面或 ~/Screenshots）
mkdir -p ~/Screenshots
defaults write com.apple.screencapture location ~/Screenshots

# 截图不要阴影
defaults write com.apple.screencapture disable-shadow -bool true

# 截图文件格式（可选：png, jpg, pdf, bmp）
defaults write com.apple.screencapture type -string "png"

killall SystemUIServer
```

---

## 🔲 热角（Hot Corners）

```bash
# 设置右下角为"快速进入屏保"（避免会议时尴尬）
# 各角编号：左上=0, 左下=1, 右上=2, 右下=3
# 动作值: 开始屏保=5, 锁定屏幕=10, 启动Mission Control=2, 桌面=4, Launchpad=11
defaults write com.apple.dock wvous-br-corner -int 3      # 右下角
defaults write com.apple.dock wvous-br-modifier -int 0     # 无修饰键
defaults write com.apple.dock wvous-br-corner -int 5       # 启动屏保

killall Dock
```

---

## ⚠️ 关闭确认弹窗

```bash
# 关闭文件扩展名更改警告
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false

# 从垃圾桶清空时不要确认
defaults write com.apple.finder WarnOnEmptyTrash -bool false

# 关闭"退出应用时确认"
defaults write com.apple.QuitAlwaysConfirm -bool false  # 部分 app 支持

killall Finder
```

---

## 🔋 电池 & 性能

```bash
# 查看电池健康状态
system_profiler SPPowerDataType | grep -E "Condition|Cycle Count|Maximum Capacity"

# 查看 CPU 架构
uname -m
# arm64 = Apple Silicon, x86_64 = Intel

# 查看当前功耗（Apple Silicon）
pmset -g batt

# 高性能模式（Intel Mac 上可选）
sudo pmset -a highperformance 1

# 禁用休眠（当合盖时不进深度睡眠，适合下载/编译场景）
sudo pmset -a hibernatemode 0
```

---

## 🧹 其他实用 tweaks

```bash
# 自动隐藏 Dock
defaults write com.apple.dock autohide -bool true
killall Dock

# 扩大点击热区（Dock 自动隐藏后反应速度）
defaults write com.apple.dock autohide-delay -float 0
defaults write com.apple.dock autohide-time-modifier -float 0.4

# 菜单栏电池显示百分比
defaults write com.apple.menuextra.battery ShowPercent -string "YES"

# 禁用"摇动鼠标以定位指针"（容易误触）
defaults write -g CGDisableCursorLocationMagnification -bool true
```

---

## ✅ 验证清单

| 设置 | 验证方式 |
|------|---------|
| 隐藏文件可见 | 在 Finder 中按 `Cmd+Shift+.` 切换 |
| 路径栏 | Finder → 显示 → 显示路径栏 |
| Xcode CLI | `gcc --version` 正常输出 |
| 按键重复 | 按住任意字符键，应快速连续输入 |
| 截图位置 | 截图后文件出现在指定目录 |
| 热角 | 鼠标移到右下角触屏保 |

---

## 🔗 参考

- `man defaults`
- `man pmset`
- https://macos-defaults.com/
