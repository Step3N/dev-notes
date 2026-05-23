# Nginx 入门配置

## 安装

### macOS

```bash
brew install nginx
```

配置文件路径：`/opt/homebrew/etc/nginx/nginx.conf`（Apple Silicon）
旧版 Intel Mac：`/usr/local/etc/nginx/nginx.conf`

### Linux

```bash
# Debian / Ubuntu
sudo apt update && sudo apt install nginx -y

# RHEL / CentOS / Fedora
sudo dnf install nginx -y
```

配置文件路径：`/etc/nginx/nginx.conf`
站点配置目录：`/etc/nginx/sites-available/` 和 `/etc/nginx/sites-enabled/`

### Windows

从 [nginx.org](https://nginx.org/) 下载 — 主要用于本地开发/测试，生产不建议。

## 验证安装

```bash
nginx -v          # 查看版本
sudo nginx -t     # 测试配置文件语法（重要！任何修改后都执行）
```

## 常用命令

```bash
sudo nginx              # 启动
sudo nginx -s stop      # 强制停止
sudo nginx -s quit      # 优雅停止（处理完当前请求）
sudo nginx -s reload    # 重载配置（不中断服务）
sudo nginx -s reopen    # 重新打开日志文件
```

### Linux 使用 systemd

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl enable nginx   # 开机自启
sudo systemctl status nginx
```

## 概念速览

| 指令 | 作用 |
|------|------|
| `http {}` | HTTP 全局配置 |
| `server {}` | 虚拟主机（一个 server 块 = 一个站点） |
| `location {}` | URL 路径匹配规则 |
| `upstream {}` | 负载均衡后端列表 |
| `events {}` | 事件驱动模型配置 |

配置文件的典型结构：

```nginx
events {
    worker_connections 1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    server {
        listen 80;
        server_name example.com;
        root /var/www/html;
    }
}
```

## 示例 1：静态文件服务器

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    # 静态资源缓存
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
}
```

## 示例 2：反向代理（前后端分离开发用最多）

```nginx
server {
    listen 80;
    server_name dev.local;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /api/ {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
    }
}
```

## 示例 3：反向代理 + 负载均衡

```nginx
upstream backend {
    server 127.0.0.1:3000 weight=3;
    server 127.0.0.1:3001;
    server 127.0.0.1:3002 backup;
}

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
    }
}
```

## 示例 4：HTTPS + Let's Encrypt

```bash
# 安装 Certbot
# macOS
brew install certbot
# Linux
sudo apt install certbot python3-certbot-nginx -y

# 获取证书并自动配置
sudo certbot --nginx -d example.com

# 测试自动续期
sudo certbot renew --dry-run

# Certbot 会自动添加定时任务，不需要手动 cron
```

自动生成的配置类似：

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # 现代 SSL 配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;

    location / {
        root /var/www/html;
    }
}

server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}
```

## 安全 Headers

```nginx
server {
    # XSS 保护
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;

    # HSTS（启用后浏览器强制 HTTPS）
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # 禁用服务器版本号泄露
    server_tokens off;
}
```

## 常见 Location 模式

```nginx
# 精确匹配
location = /favicon.ico {
    log_not_found off;
    access_log off;
}

# 前缀匹配
location /static/ {
    alias /var/www/static/;
}

# 正则匹配
location ~ \.php$ {
    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}

# 优先级：= > ^~ > 正则(~ / ~*) > 前缀
```

## 调试与排错

```bash
# 1. 测试配置文件
sudo nginx -t

# 2. 查看错误日志
sudo tail -f /var/log/nginx/error.log          # Linux
sudo tail -f /opt/homebrew/var/log/nginx/error.log  # macOS brew

# 3. 检查端口占用
sudo lsof -i :80
sudo lsof -i :443

# 4. 防火墙检查（Linux）
sudo ufw status
sudo ufw allow 'Nginx Full'

# 5. SELinux（Linux）
getsebool -a | grep httpd
sudo setsebool -P httpd_can_network_connect 1   # 允许反向代理
```

## 快速验证

```bash
# 创建测试用静态页面
echo "<h1>Nginx 工作正常</h1>" > /tmp/test.html

# 用 nginx 容器测试配置（不需要安装）
docker run --rm -v /tmp/test.html:/usr/share/nginx/html/test.html:ro -p 8080:80 nginx:alpine

# 访问验证
curl http://localhost:8080/test.html
```

> **平台差异**：Nginx 主要面向 Linux 生产环境。macOS 通过 Homebrew 安装，配置路径略微不同。Windows 版本功能完整但性能较低，适合本地测试。配置文件语法全平台一致，注意路径分隔符和文件系统权限差异。
