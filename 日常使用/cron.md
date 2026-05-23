# Cron 定时任务

> **平台**：全平台（macOS/Linux 用 cron，Windows 用任务计划程序）

## Cron 语法（macOS / Linux）

```text
* * * * * command
─ ─ ─ ─ ─
│ │ │ │ └── 星期 (0-7, 0=周日)
│ │ │ └──── 月份 (1-12)
│ │ └────── 日期 (1-31)
│ └──────── 小时 (0-23)
└────────── 分钟 (0-59)
```

### 常用时间

```text
0 2 * * *          每天 2:00
0 9 * * 1          每周一 9:00
*/30 * * * *       每 30 分钟
*/15 9-18 * * 1-5  工作日 9-18 点每 15 分钟
0 0 1 * *          每月 1 号 0:00
@daily             每天 0:00（等效 0 0 * * *）
@reboot            系统启动时
```

## crontab 命令

```bash
crontab -e           # 编辑
crontab -l           # 列出
crontab -r           # 删除

# 指定编辑器
EDITOR=vim crontab -e
EDITOR="code --wait" crontab -e
```

## 实用示例

```bash
# 每天早上 9:00 git pull
0 9 * * * cd /home/user/project && git pull >> ~/logs/git-pull.log 2>&1

# 每天 3:00 数据库备份
0 3 * * * mysqldump -u root mydb | gzip > /backup/mydb_$(date +\%Y\%m\%d).sql.gz

# 每天 2:00 清理 7 天前临时文件
0 2 * * * find /tmp -type f -mtime +7 -delete >> /var/log/cleanup.log 2>&1

# 每 5 分钟健康检查
*/5 * * * * curl -sS -o /dev/null http://localhost/health || systemctl restart nginx

# 每周日 0:00 清理 30 天前日志
0 0 * * 0 find /var/log/myapp -name "*.log" -mtime +30 -delete
```

## 日志与调试

```bash
# macOS
grep -i cron /var/log/system.log

# Linux
grep CRON /var/log/syslog
# 或
journalctl -u cron

# 查看 cron 是否运行
ps aux | grep cron
systemctl status cron   # Linux
```

## 环境变量问题

cron 的 shell 环境非常精简，不加载 `.zshrc`：

```bash
# 在 crontab 顶部设置
SHELL=/bin/bash
PATH=/usr/local/bin:/usr/bin:/bin

# 备份脚本
0 2 * * * /usr/local/bin/backup.sh
```

```bash
# backup.sh 内容
#!/bin/bash
export PATH=/usr/local/bin:/usr/bin:/bin
mysqldump -u root mydb > /backup/db.sql
```

## macOS：cron vs launchd

cron 在 macOS 上可用。macOS 更推荐 `launchd`，但 cron 简单够用：

```bash
launchctl list   # 查看 launchd 任务
```

## Windows：任务计划程序

### 图形界面

```
任务计划程序 → 创建基本任务 → 名称/触发器/操作
```

### 命令行（schtasks）

```powershell
# 每天 2:00 执行
schtasks /create /tn "DailyBackup" /tr "C:\backup.bat" /sc daily /st 02:00

# 列出
schtasks /query /fo LIST /v

# 运行 / 删除
schtasks /run /tn "DailyBackup"
schtasks /delete /tn "DailyBackup" /f
```

## 快速测试

```bash
# 每分钟写时间到文件（测试用）
echo "* * * * * echo $(date) >> ~/cron-test.log" | crontab -
cat ~/cron-test.log   # 1 分钟后查看
crontab -r             # 清理
```

---

**参考**：
- crontab.guru: https://crontab.guru
- Windows schtasks: `schtasks /?`
