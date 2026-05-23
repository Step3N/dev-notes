# API Key 管理最佳实践

**绝不把 API Key 硬编码在代码里，也绝不提交到 Git。**

---

## 核心原则

1. 代码不包含任何密钥
2. 密钥通过外部配置注入
3. 每个环境使用不同密钥
4. 密钥泄露后立即轮换

---

## 方式一：环境变量（推荐）

```bash
# ~/.zshrc 或 ~/.bashrc
export OPENAI_API_KEY="sk-your-key-here"
export ANTHROPIC_API_KEY="sk-ant-your-key-here"
```

Python 中读取：

```python
import os

openai_key = os.getenv("OPENAI_API_KEY")
if not openai_key:
    raise ValueError("OPENAI_API_KEY 未设置")
```

大多数 SDK 会自动读取约定好的环境变量名：
- OpenAI → `OPENAI_API_KEY`
- Anthropic → `ANTHROPIC_API_KEY`
- DeepSeek → `DEEPSEEK_API_KEY`

---

## 方式二：.env 文件（仅本地开发）

```
# .env — 此文件必须在 .gitignore 中
OPENAI_API_KEY=sk-your-key-here
ANTHROPIC_API_KEY=sk-ant-your-key-here
OPENAI_BASE_URL=https://api.openai.com/v1
```

```bash
pip install python-dotenv
```

```python
from dotenv import load_dotenv
import os

load_dotenv()  # 加载 .env 文件

client = OpenAI()  # 自动读取 OPENAI_API_KEY
```

**.gitignore 必须包含：**
```gitignore
.env
.env.*
```

> 注意：`.env.example` 可以提交到 git，里面放空值或说明。`.env` 本身绝不提交。

---

## 方式三：密码管理器 CLI

### 1Password CLI

```bash
# 安装 op CLI: https://developer.1password.com/docs/cli/

# 在 shell 配置里注入
eval $(op signin --account my.1password.com)

# 运行命令时自动注入密钥
op run -- python app.py
```

### Bitwarden CLI

```bash
# 安装: brew install bitwarden-cli
bw login
export BW_SESSION=$(bw unlock --raw)

# 获取密钥
bw get item "OpenAI API Key" | jq -r '.login.password'
```

---

## 方式四：操作系统密钥链

### macOS Keychain

```bash
# 写入
security add-generic-password -s "openai-api-key" -a "$USER" -w "sk-your-key"

# 读取
security find-generic-password -s "openai-api-key" -a "$USER" -w
```

Python 中使用：

```python
import subprocess

def get_key(service, account):
    cmd = f'security find-generic-password -s "{service}" -a "{account}" -w'
    return subprocess.check_output(cmd, shell=True).decode().strip()
```

### keyring Python 库

```python
import keyring

# 写入
keyring.set_password("openai", "api_key", "sk-your-key")

# 读取
key = keyring.get_password("openai", "api_key")
```

---

## 方式五：CI/CD Secrets

### GitHub Actions

```yaml
# .github/workflows/deploy.yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: python script.py
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

Settings → Secrets and variables → Actions 中添加。

---

## 杜绝的坏习惯

| ❌ 错误做法 | ✅ 正确做法 |
|------------|------------|
| `api_key = "sk-xxx"` 写在代码里 | 用环境变量或密钥管理工具 |
| `.env` 提交到 git | `.env` 在 `.gitignore`，提交 `.env.example` |
| API key 截图发给别人 | 直接复制并告知对方用完即删 |
| 日志里打印环境变量 | 日志脱敏，禁止打印密钥 |
| 多个项目共用一个 key | 每个项目或环境使用独立 key |

---

## 密钥泄露了怎么办

1. **立即轮换**：登录对应平台删除旧 key，生成新 key
2. **检查用量**：查看是否有异常调用
3. **检查日志/审计**：看泄露的 key 是否被滥用
4. **修复泄露源**：检查 git 历史、环境变量、日志等

OpenAI 撤销 key：https://platform.openai.com/api-keys
Anthropic 撤销 key：https://console.anthropic.com/settings/keys

---

## 平台

全平台适用。
