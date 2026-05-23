# cURL 速查表

> 平台：全平台（macOS/Linux 预装，Windows 需安装）

## 基本用法

| 命令 | 说明 |
|------|------|
| `curl https://api.example.com` | GET 请求，输出响应体 |
| `curl -i https://api.example.com` | 包含响应头 |
| `curl -v https://api.example.com` | 详细输出（握手/请求/响应） |
| `curl -o file.json https://api.example.com` | 保存到文件 |
| `curl -O https://example.com/file.zip` | 用远程文件名保存 |

## HTTP 方法

| 命令 | 说明 |
|------|------|
| `curl -X GET https://...` | GET（默认，可省略） |
| `curl -X POST https://...` | POST |
| `curl -X PUT https://...` | PUT |
| `curl -X PATCH https://...` | PATCH |
| `curl -X DELETE https://...` | DELETE |

## 请求头

| 命令 | 说明 |
|------|------|
| `curl -H "Content-Type: application/json" ...` | 自定义头 |
| `curl -H "Authorization: Bearer <token>" ...` | Bearer Token |
| `curl -H "Accept: application/json" ...` | 接受类型 |
| `curl -H "User-Agent: curl/8.0" ...` | 自定义 UA |

## 发送数据

| 命令 | 说明 |
|------|------|
| `curl -d "name=foo&age=18" ...` | URL 编码表单 |
| `curl -d '{"key":"value"}' -H "Content-Type: application/json" ...` | JSON 体 |
| `curl --data-binary @file.json ...` | 发送文件内容 |
| `curl -F "file=@photo.jpg" ...` | multipart 表单上传 |

## JSON API 示例

```bash
# GET 数据
curl https://jsonplaceholder.typicode.com/posts/1

# POST 创建
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"foo","body":"bar","userId":1}'

# PUT 更新
curl -X PUT https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"id":1,"title":"updated","body":"new body","userId":1}'

# DELETE
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

## 认证

| 命令 | 说明 |
|------|------|
| `curl -u user:pass https://...` | Basic Auth |
| `curl -H "Authorization: Bearer <token>" ...` | Bearer Token |
| `curl --oauth2-bearer <token> ...` | OAuth2 简写 |
| `curl --netrc` | 使用 `~/.netrc` 凭据 |

## Cookie

| 命令 | 说明 |
|------|------|
| `curl -b "name=value" ...` | 发送 Cookie |
| `curl -b cookies.txt -c cookies.txt ...` | 读取 + 保存 Cookie |

## 文件传输

| 命令 | 说明 |
|------|------|
| `curl -T local.txt ftp://server/` | FTP 上传 |
| `curl -u user:pass ftp://server/file` | FTP 下载 |
| `curl -O https://example.com/file.zip` | HTTP 下载 |
| `curl -C - -O https://.../large.zip` | 断点续传 |

## 实用选项

| 命令 | 说明 |
|------|------|
| `curl -L https://...` | 跟随重定向（默认不跟随） |
| `curl --max-time 10 https://...` | 超时 10 秒 |
| `curl --connect-timeout 5 https://...` | 连接超时 5 秒 |
| `curl -s https://...` | 静默模式（无进度条/错误） |
| `curl -sS https://...` | 静默但显示错误 |
| `curl -k https://...` | 跳过 SSL 验证 |
| `curl --retry 3 https://...` | 失败重试 3 次 |
| `curl -w "\n%{http_code}\n" ...` | 输出 HTTP 状态码 |
| `curl -x http://proxy:8080 https://...` | 通过代理 |

## 快捷别名 (建议添加至 shell rc)

```bash
alias api-get='curl -s -H "Accept: application/json"'
alias api-post='curl -s -X POST -H "Content-Type: application/json"'
alias api-json='curl -s -H "Content-Type: application/json" -H "Accept: application/json"'
```
