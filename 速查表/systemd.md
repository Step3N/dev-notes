# systemd 速查表

> 平台：Linux（主流发行版）

## Service 管理

| 命令 | 说明 |
|------|------|
| `systemctl start <service>` | 启动服务 |
| `systemctl stop <service>` | 停止服务 |
| `systemctl restart <service>` | 重启服务 |
| `systemctl reload <service>` | 重载配置（不中断） |
| `systemctl enable <service>` | 开机自启 |
| `systemctl disable <service>` | 禁用开机自启 |
| `systemctl status <service>` | 查看状态（含最近日志） |
| `systemctl is-active <service>` | 是否运行中 |
| `systemctl is-enabled <service>` | 是否启用自启 |
| `systemctl mask <service>` | 彻底禁用（无法手动启动） |
| `systemctl unmask <service>` | 取消屏蔽 |

## Systemctl 查询

| 命令 | 说明 |
|------|------|
| `systemctl list-units` | 列出所有活跃单元 |
| `systemctl list-units --all` | 列出所有单元（含 inactive） |
| `systemctl list-units --type=service` | 仅 service 类型 |
| `systemctl list-units --state=running` | 仅运行中的单元 |
| `systemctl list-unit-files` | 列出所有单元文件及启用状态 |
| `systemctl list-dependencies <service>` | 查看依赖树 |
| `systemctl daemon-reload` | 重载单元文件（修改后必须执行） |

## Journalctl（日志）

| 命令 | 说明 |
|------|------|
| `journalctl` | 查看所有日志（分页） |
| `journalctl -u <service>` | 查看指定服务日志 |
| `journalctl -u <service> -f` | 实时跟踪日志（类似 tail -f） |
| `journalctl -u <service> --since "1 hour ago"` | 最近 1 小时 |
| `journalctl -u <service> --since "2024-01-01" --until "2024-01-02"` | 时间范围 |
| `journalctl -p err -b` | 本次启动的错误日志 |
| `journalctl -k` | 内核日志 |
| `journalctl --vacuum-size=200M` | 限制日志占用 200MB |
| `journalctl --vacuum-time=14d` | 保留最近 14 天 |
| `journalctl -o json-pretty` | JSON 格式输出 |

## 单元文件位置

| 路径 | 说明 |
|------|------|
| `/etc/systemd/system/` | 用户自定义（优先级最高） |
| `/run/systemd/system/` | 运行时单元（重启丢失） |
| `/usr/lib/systemd/system/` | 发行版打包安装的默认单元 |
| `/etc/systemd/system/<name>.d/` | 覆盖配置目录 |

## 单元文件结构

```ini
[Unit]
Description=My Service
After=network.target
Wants=redis.service

[Service]
Type=simple
ExecStart=/usr/local/bin/my-app --config /etc/my-app.yml
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=5
User=myapp
Group=myapp
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target
```

## Timer 单元（cron 替代）

**timer 文件** `/etc/systemd/system/backup.timer`：

```ini
[Unit]
Description=每日备份

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

**配套 service 文件** `/etc/systemd/system/backup.service`：

```ini
[Unit]
Description=备份脚本

[Service]
ExecStart=/usr/local/bin/backup.sh
```

| 命令 | 说明 |
|------|------|
| `systemctl start backup.timer` | 启动定时器 |
| `systemctl enable backup.timer` | 启用定时器 |
| `systemctl list-timers` | 列出所有定时器 |
| `systemctl list-timers --all` | 含已失效的定时器 |

**OnCalendar 格式示例：**

| 表达式 | 含义 |
|--------|------|
| `daily` | 每天 00:00 |
| `hourly` | 每小时整点 |
| `weekly` | 每周一 00:00 |
| `Mon..Fri 09:00:00` | 工作日 9 点 |
| `*-*-01,15 00:00:00` | 每月 1 号和 15 号 |
| `*:0/15` | 每 15 分钟 |

## 快速创建服务

```bash
# 1. 创建单元文件
sudo tee /etc/systemd/system/myapp.service <<'EOF'
[Unit]
Description=My App
After=network.target

[Service]
ExecStart=/usr/bin/node /opt/myapp/index.js
Restart=always
User=deploy

[Install]
WantedBy=multi-user.target
EOF

# 2. 重载并启动
sudo systemctl daemon-reload
sudo systemctl enable --now myapp

# 3. 查看状态
systemctl status myapp
```
