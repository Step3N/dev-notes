# Go / Rust / Java — 快速安装与入门

三大主流编程语言的环境配置，一篇搞定。

---

# Go

## 📦 安装

### macOS

```bash
brew install go
```

### Windows

```powershell
winget install GoLang.Go
```

### Linux — 方式一：包管理器

```bash
# Ubuntu / Debian
sudo apt install golang-go

# Fedora
sudo dnf install golang

# Arch
sudo pacman -S go
```

> ⚠️ 包管理器版本可能过旧，推荐方式二。

### Linux — 方式二：官方 tar.gz

```bash
# 从 https://go.dev/dl/ 获取最新版本号，替换下面的版本号
wget https://go.dev/dl/go1.23.0.linux-amd64.tar.gz

# 解压到 /usr/local
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz
```

## ✅ 验证

```bash
go version
# 输出示例: go version go1.23.0 darwin/amd64
```

## ⚙️ 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `GOROOT` | go 安装路径 | Go 编译器和标准库位置 |
| `GOPATH` | `~/go` | 工作区（第三方包、编译输出） |
| `GOBIN` | `$GOPATH/bin` | 编译后的二进制文件存放位置 |

查看：

```bash
go env GOROOT GOPATH GOBIN
```

## 🔧 常用命令

```bash
go version                    # 版本
go run main.go                # 编译并运行
go build                      # 编译当前包
go build -o app .             # 指定输出文件名
go test ./...                 # 运行所有测试
go mod init <module-name>     # 初始化模块
go mod tidy                   # 整理依赖
go get <pkg>                  # 添加依赖
go install <pkg>              # 下载并安装
```

快速体验：

```bash
mkdir hello && cd hello
go mod init hello

cat > main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
EOF

go run main.go
```

---

# Rust

## 📦 安装（全平台统一）

通过 **rustup** 安装，推荐方式：

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装交互中选 **1）default**。

装完后重启终端或手动加载：

```bash
source "$HOME/.cargo/env"
```

### Windows 额外步骤

1. 下载并运行 [rustup-init.exe](https://rustup.rs)
2. 安装 **Visual Studio Build Tools**（提示时选 "Desktop development with C++"）
   - 或单独安装：https://visualstudio.microsoft.com/visual-cpp-build-tools/

## ✅ 验证

```bash
rustc --version
# 输出示例: rustc 1.81.0 (eeb90cda1 2024-09-04)

cargo --version
# 输出示例: cargo 1.81.0
```

## 🛠 Cargo — Rust 包管理器 + 构建工具

```bash
cargo new my-app              # 创建新项目
cd my-app
cargo build                   # 编译（debug 模式）
cargo build --release         # 编译（release 模式，优化）
cargo run                     # 编译并运行
cargo check                   # 只检查语法不生成二进制（最快）
cargo test                    # 运行测试
cargo add serde               # 添加依赖
cargo remove serde            # 移除依赖
cargo update                  # 更新依赖
cargo fmt                     # 格式化代码
cargo clippy                  # Lint 检查
```

### 添加国内镜像（crates.io 代理）

编辑 `~/.cargo/config.toml`：

```toml
[source.crates-io]
replace-with = "ustc"

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"
```

快速体验：

```bash
cargo new hello-rust
cd hello-rust
cargo run
# 输出: Hello, world!
```

### 更新 Rust

```bash
rustup update
rustup self uninstall        # 卸载
```

---

# Java

## 📦 JDK 选择

| JDK | 说明 | 推荐 |
|-----|------|------|
| **Oracle JDK** | 官方版，商业使用需付费（17+ 免费） | 不推荐 |
| **OpenJDK** | 开源实现 | ✅ 推荐 |
| **Adoptium (Eclipse Temurin)** | 最流行的 OpenJDK 构建 | ✅ 新手首选 |
| **Amazon Corretto** | AWS 维护，长期支持 | 备选 |
| **GraalVM** | 高性能，支持 Native Image | 进阶 |

## 🛠 sdkman — JDK 版本管理（macOS / Linux / WSL）

sdkman 是最灵活的 Java 版本管理工具。

### 安装 sdkman

```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

验证：

```bash
sdk version
```

### 使用 sdkman 管理 JDK

```bash
# 列出可用 JDK 版本
sdk list java

# 安装指定版本
sdk install java 17.0.12-tem

# 切换版本
sdk use java 21.0.4-tem

# 设置默认
sdk default java 17.0.12-tem

# 查看当前版本
sdk current java
```

### macOS — Homebrew 安装

```bash
brew install openjdk@17
brew install openjdk@21
```

### Windows — winget 安装

```powershell
# Eclipse Temurin
winget install EclipseAdoptium.Temurin.17.JDK
winget install EclipseAdoptium.Temurin.21.JDK
```

### Linux — 包管理器

```bash
# Ubuntu / Debian
sudo apt install openjdk-17-jdk

# Fedora
sudo dnf install java-17-openjdk-devel

# Arch
sudo pacman -S jdk17-openjdk
```

## ✅ 验证

```bash
java --version
# 输出示例:
# openjdk 17.0.12 2024-07-16 LTS
# OpenJDK 64-Bit Server VM (build 17.0.12+7-LTS, mixed mode)

javac --version
# 输出示例: javac 17.0.12
```

## ⚙️ 设置 JAVA_HOME

### macOS（sdkman）

sdkman 自动设置，无需手动。

### macOS（Homebrew）

```bash
echo 'export JAVA_HOME=$(/usr/libexec/java_home -v 17)' >> ~/.zshrc
source ~/.zshrc
```

### Windows

```powershell
# 系统属性 → 环境变量 → 新建
# 变量名: JAVA_HOME
# 变量值: C:\Program Files\Eclipse Adoptium\jdk-17.0.12.7-hotspot\
```

或 PowerShell 设置：

```powershell
[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Eclipse Adoptium\jdk-17.0.12.7-hotspot\", "User")
```

### Linux

```bash
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.zshrc
source ~/.zshrc
```

验证：

```bash
echo $JAVA_HOME
```

---

## 🔗 参考

- Go: https://go.dev
- Rust: https://rustup.rs
- Cargo 镜像: https://mirrors.ustc.edu.cn/help/crates.io-index.html
- sdkman: https://sdkman.io
- Adoptium: https://adoptium.net
