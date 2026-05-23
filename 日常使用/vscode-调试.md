# VS Code 调试配置（launch.json）

全平台通用。

---

## 什么是 launch.json

`launch.json` 是 VS Code 的调试配置文件，位于项目根目录的 `.vscode/launch.json`。它告诉 VS Code **如何启动调试器**。

### 创建方式

1. 打开 Run 面板（`⌘⇧D` / `Ctrl+Shift+D`）
2. 点击 **create a launch.json file**
3. 选择调试环境（Node.js / Python / Chrome 等）

VS Code 会自动生成 `.vscode/launch.json` 文件。

---

## 基础结构

```jsonc
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "启动配置名称",         // 显示在 Run 下拉菜单中
      "type": "node",               // 调试器类型
      "request": "launch",          // launch: 启动程序 | attach: 附加到已运行进程
      "program": "${workspaceFolder}/index.js",
      "args": [],                   // 命令行参数
      "env": {},                    // 环境变量
      "cwd": "${workspaceFolder}",  // 工作目录
      "preLaunchTask": "",          // 调试前运行的任务（如编译）
      "stopOnEntry": false,         // 是否在入口处暂停
    }
  ],
  "compounds": []                   // 组合多个配置同时调试
}
```

### 常用变量

| 变量 | 说明 |
|------|------|
| `${workspaceFolder}` | 当前项目根目录 |
| `${file}` | 当前打开的文件 |
| `${fileBasename}` | 当前文件名（不含路径） |
| `${fileDirname}` | 当前文件所在目录 |
| `${env:VAR_NAME}` | 环境变量值 |

---

## Node.js 调试

### 启动当前文件

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Node.js 启动当前文件",
      "type": "node",
      "request": "launch",
      "program": "${file}",
      "skipFiles": ["<node_internals>/**"]
    }
  ]
}
```

### 启动 npm 脚本

```json
{
  "name": "启动 npm dev",
  "type": "node",
  "request": "launch",
  "runtimeExecutable": "npm",
  "runtimeArgs": ["run", "dev"],
  "console": "integratedTerminal"
}
```

### Attach 到已运行的 Node 进程

```json
{
  "name": "Attach 到进程",
  "type": "node",
  "request": "attach",
  "port": 9229,
  "restart": true
}
```

启动目标进程时需要加 `--inspect` 参数：

```bash
node --inspect app.js
```

---

## Python 调试

需要安装 Python 扩展（`ms-python.python`）。

### 启动当前文件

```json
{
  "name": "Python: 当前文件",
  "type": "python",
  "request": "launch",
  "program": "${file}",
  "console": "integratedTerminal"
}
```

### 启动模块

```json
{
  "name": "Python: 模块",
  "type": "python",
  "request": "launch",
  "module": "flask.run",
  "args": ["run", "--no-debugger"],
  "jinja": true
}
```

### Django

```json
{
  "name": "Django",
  "type": "python",
  "request": "launch",
  "program": "${workspaceFolder}/manage.py",
  "args": ["runserver"],
  "django": true
}
```

### Flask

```json
{
  "name": "Flask",
  "type": "python",
  "request": "launch",
  "module": "flask",
  "args": ["run", "--no-debugger"],
  "env": {
    "FLASK_APP": "${workspaceFolder}/app.py",
    "FLASK_ENV": "development"
  }
}
```

---

## Chrome 调试（前端）

需要安装 **Debugger for Chrome** 扩展（`msjsdiag.debugger-for-chrome`）或 VS Code 内置 JS Debugger。

```json
{
  "name": "Launch Chrome",
  "type": "chrome",
  "request": "launch",
  "url": "http://localhost:3000",
  "webRoot": "${workspaceFolder}",
  "sourceMapPathOverrides": {
    "webpack:///src/*": "${webRoot}/*"
  }
}
```

配合 `preLaunchTask` 先启动开发服务器：

```json
{
  "name": "启动 Chrome + 服务器",
  "type": "chrome",
  "request": "launch",
  "url": "http://localhost:3000",
  "webRoot": "${workspaceFolder}",
  "preLaunchTask": "npm: dev"
}
```

---

## Compound（多目标调试）

```json
{
  "version": "0.2.0",
  "configurations": [
    { "name": "Server", "type": "node", "request": "launch", "program": "${workspaceFolder}/server.js" },
    { "name": "Client", "type": "chrome", "request": "launch", "url": "http://localhost:8080", "webRoot": "${workspaceFolder}" }
  ],
  "compounds": [
    {
      "name": "Server + Client",
      "configurations": ["Server", "Client"]
    }
  ]
}
```

---

## 调试技巧

| 操作 | 快捷键 | 说明 |
|------|--------|------|
| 设置断点 | `F9` | 单击行号左侧 |
| 条件断点 | 右键断点 → Edit Breakpoint | 设置表达式/命中次数 |
| 日志断点 | 右键断点 → Log Message | 不中断，输出日志 |
| Watch | 在 Debug 视图添加变量 | 监控表达式值 |
| 调用堆栈 | Debug 视图左侧面板 | 查看函数调用链 |
| 变量面板 | Debug 视图 | 查看/修改变量值 |

### 条件断点示例

```jsonc
// 右键断点 → "Edit Breakpoint"
// 条件：i > 10
// 命中次数：>= 3
// 日志：循环中 i = {i}
```

---

## 配置文件选择

在 `.vscode/launch.json` 中可添加多个配置，通过 Run 面板下拉菜单选择使用哪个。

也可在 `settings.json` 中设置默认配置：

```json
{
  "launch.defaultConfiguration": "启动 npm dev"
}
```

---

## 常见问题

- **调试器无法附加**：确认目标进程以 debug 模式启动（`--inspect`、`--inspect-brk`）
- **断点未命中**：检查 source map 配置、`"outFiles"` 是否正确
- **环境变量不生效**：在 `env` 或 `envFile` 中设置，`envFile` 使用 `.env` 格式
