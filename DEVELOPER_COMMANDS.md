# OpenClaw 开发者命令参考

## 项目设置

### 安装依赖

```bash
pnpm install
```

### 构建项目

```bash
pnpm build
```

### 类型检查和代码质量

```bash
pnpm check          # 运行所有检查（格式化、类型检查、lint）
pnpm format         # 格式化代码
pnpm format:check   # 检查格式化
pnpm tsgo           # TypeScript 类型检查
pnpm lint           # 代码检查
```

## 开发服务器

### UI 开发服务器

```bash
pnpm ui:dev         # 启动 UI 开发服务器 (http://localhost:5173/)
```

### 网关开发服务器

```bash
pnpm gateway:dev    # 启动网关开发服务器
```

### CLI 开发模式

```bash
pnpm dev            # 运行 CLI 开发模式
```

## 测试

### 运行测试

```bash
pnpm test           # 运行所有测试
pnpm test:fast      # 快速运行单元测试
pnpm test:e2e       # 运行端到端测试
pnpm test:live      # 运行实时测试（需要真实密钥）
pnpm test:coverage  # 运行测试并生成覆盖率报告
```

### 特定测试

```bash
pnpm test:channels  # 运行频道相关测试
pnpm test:gateway   # 运行网关测试
pnpm test:extensions # 运行扩展测试
```

## Docker 测试

```bash
pnpm test:docker:all           # 运行所有 Docker 测试
pnpm test:docker:live-models   # Docker 实时模型测试
pnpm test:docker:live-gateway  # Docker 实时网关测试
pnpm test:docker:onboard       # Docker 入门测试
```

## 移动端开发

### iOS

```bash
pnpm ios:build      # 构建 iOS 应用
pnpm ios:gen        # 生成 Xcode 项目
pnpm ios:open       # 打开 Xcode 项目
pnpm ios:run        # 构建并运行 iOS 应用
```

### Android

```bash
pnpm android:assemble  # 构建 Android APK
pnpm android:install   # 安装 Android 应用
pnpm android:run       # 安装并运行 Android 应用
pnpm android:test      # 运行 Android 单元测试
```

## macOS 应用

```bash
pnpm mac:package    # 打包 macOS 应用
pnpm mac:open       # 打开打包的 macOS 应用
pnpm mac:restart    # 重启 macOS 应用
```

## 文档

```bash
pnpm docs:dev       # 启动文档开发服务器
pnpm docs:check-links # 检查文档链接
pnpm docs:spellcheck # 拼写检查
```

## 插件和扩展

```bash
pnpm plugins:sync   # 同步插件版本
```

## 协议生成

```bash
pnpm protocol:gen   # 生成协议文件
pnpm protocol:check # 检查协议文件是否最新
```

## 发布检查

```bash
pnpm release:check  # 发布前检查
```

## 常用工作流

### 1. 启动完整开发环境

```bash
# 终端1：启动网关
pnpm gateway:dev

# 终端2：启动 UI
pnpm ui:dev
```

### 2. 运行测试套件

```bash
# 安装依赖（如果需要）
pnpm install

# 运行所有检查
pnpm check

# 运行测试
pnpm test
```

### 3. 构建和打包

```bash
# 构建项目
pnpm build

# 打包 macOS 应用
pnpm mac:package

# 构建 UI
pnpm ui:build
```

## 故障排除

### 依赖问题

```bash
# 清理并重新安装
rm -rf node_modules
pnpm install
```

### 构建失败

```bash
# 检查类型错误
pnpm tsgo

# 检查代码格式
pnpm format:check
```

### UI 开发服务器无法启动

```bash
# 确保在项目根目录
cd /Volumes/NodeJsProduct/github-test/openclaw-main

# 安装 UI 依赖
pnpm ui:install

# 启动开发服务器
pnpm ui:dev
```

## 环境变量

### 开发环境

```bash
OPENCLAW_PROFILE=dev        # 开发模式
OPENCLAW_SKIP_CHANNELS=1    # 跳过频道初始化（加快启动）
CLAWDBOT_SKIP_CHANNELS=1    # 跳过 ClawdBot 频道初始化
```

### 开发者模式运行正式环境 Onboard

在开发者模式下（使用 `pnpm dev`）运行正式环境的 `openclaw onboard`，需要使用 `--profile` 标志指定正式环境配置：

```bash
# 方法1：使用 --profile 指定正式环境（推荐）
pnpm dev onboard --profile default

# 方法2：完整参数指定正式环境配置目录
OPENCLAW_STATE_DIR=~/.openclaw pnpm dev onboard

# 非交互式 onboard 示例
pnpm dev onboard --non-interactive --mode local --auth-choice setup-token

# 使用 --profile 运行 agent 命令（需要指定 --agent 参数）
pnpm dev --profile default agent --message "Hello" --agent main
```

**说明**：

- `--dev` 标志会将状态隔离到 `~/.openclaw-dev` 并使用开发端口
- `--profile <name>` 将状态隔离到 `~/.openclaw-<name>` 目录
- 使用 `--profile default` 或设置 `OPENCLAW_STATE_DIR=~/.openclaw` 可访问正式环境配置
- 详细选项参见 https://docs.openclaw.ai/cli#onboard

**注意**：在源码开发模式下运行 `onboard` 时，避免同时使用 `--dev` 标志，因为这可能导致工作区模板加载问题。如果需要重置开发环境配置，建议使用：

```bash
# 重置开发环境并重新 onboard
pnpm dev onboard --reset
```

### 开发者模式认证配置

开发者模式（`~/.openclaw-dev`）和正式环境（`~/.openclaw`）的配置是完全隔离的。如果你在正式环境配置了 API key，但在 dev 模式运行时报错 `No API key found for provider`，需要同步认证配置：

```bash
# 方法1：在开发者模式下重新运行 onboard 配置认证
pnpm dev onboard

# 方法2：手动复制正式环境的认证配置到 dev 环境
cp ~/.openclaw/openclaw.json ~/.openclaw-dev/openclaw.json

# 方法3：使用 --profile 指定使用正式环境配置（推荐用于测试）
# 启动网关
pnpm dev --profile default gateway

# 运行 agent 命令（需要指定 --agent 或 --to）
pnpm dev --profile default agent --message "Hello" --agent main
```

**检查配置**：

```bash
# 查看 dev 环境配置
cat ~/.openclaw-dev/openclaw.json | grep -A5 '"auth"'

# 查看正式环境配置
cat ~/.openclaw/openclaw.json | grep -A5 '"auth"'
```

### 测试环境

```bash
OPENCLAW_LIVE_TEST=1        # 启用实时测试
CLAWDBOT_LIVE_TEST=1        # 启用 ClawdBot 实时测试
OPENCLAW_TEST_PROFILE=low   # 低内存测试模式
```

## iOS 客户端二维码生成

为 iOS 客户端生成配对二维码和设置码。

### 基本命令

```bash
# 生成二维码（默认显示 ASCII 二维码和设置码）
openclaw qr

# 仅输出设置码（用于复制粘贴）
openclaw qr --setup-code-only

# JSON 格式输出（用于脚本处理）
openclaw qr --json

# 不显示 ASCII 二维码
openclaw qr --no-ascii
```

### 远程网关二维码

```bash
# 使用远程网关配置生成二维码
openclaw qr --remote

# 指定自定义网关 URL
openclaw qr --url ws://192.168.1.100:18789
openclaw qr --url wss://gateway.example.com/ws

# 指定公网 URL（用于设备配对插件）
openclaw qr --public-url https://ts.myhost.com

# 使用 Token 认证
openclaw qr --token <your-gateway-token>

# 使用密码认证
openclaw qr --password <your-password>
```

### 开发者模式下生成二维码

```bash
# 开发模式生成二维码（使用 dev 配置）
pnpm dev qr

# 使用正式环境配置生成二维码
pnpm dev qr --profile default

# 指定端口（如果网关非默认端口）
pnpm dev qr --url ws://127.0.0.1:19001 --token <token>

# JSON 格式输出
pnpm dev qr --json
```

### 完整工作流示例

```bash
# 1. 启动网关（如果未运行）
pnpm gateway:dev
# 或
openclaw gateway run

# 2. 生成二维码供 iOS 扫描
openclaw qr

# 输出示例：
# Pairing QR
# Scan this with the OpenClaw iOS app (Onboarding -> Scan QR).
# 
# ███████████████████████████
# █ ▄▄▄▄▄ █▀▄▀▄▀▄▀▄▀▄▀▄▀▄▀▄▀█
# ... (二维码图案) ...
# 
# Setup code: openclaw://setup?...
# Gateway: ws://192.168.1.100:18789
# Auth: token
# Source: remote

# 3. iOS 扫描后，查看待批准设备
openclaw devices list

# 4. 批准设备配对
openclaw devices approve
```

### 自动化脚本示例

```bash
# 生成二维码并提取设置码给自动化工具
#!/bin/bash
setup_code=$(openclaw qr --setup-code-only)
echo "Setup code: $setup_code"
# 可以发送到 Slack、邮件等

# 生成 JSON 格式并解析
openclaw qr --json | jq -r '.setupCode'
openclaw qr --json | jq -r '.gatewayUrl'
```

### 二维码选项速查

| 选项 | 说明 |
|------|------|
| `--remote` | 使用 `gateway.remote.url` 配置 |
| `--url <url>` | 覆盖网关 URL |
| `--public-url <url>` | 覆盖公网 URL |
| `--token <token>` | 指定 Token |
| `--password <password>` | 指定密码 |
| `--setup-code-only` | 仅输出设置码 |
| `--no-ascii` | 不显示 ASCII 二维码 |
| `--json` | JSON 格式输出 |

## 设备批准连接网关

当新设备（手机、桌面应用等）尝试连接网关时，需要批准配对请求。

### 查看待处理的配对请求

```bash
# 列出所有待处理和已配对的设备
openclaw devices list

# JSON 格式输出（用于脚本处理）
openclaw devices list --json
```

### 批准设备配对

```bash
# 批准最新的待处理请求（最常用）
openclaw devices approve

# 批准指定的请求 ID
openclaw devices approve <requestId>

# 明确指定批准最新的请求
openclaw devices approve --latest

# 批准并获取 JSON 输出（用于脚本处理）
openclaw devices approve --json
openclaw devices approve <requestId> --json
```

**完整工作流示例**：

```bash
# 1. 查看待处理请求
openclaw devices list

# 输出示例：
# Pending (1)
# Request    Device                Role       IP              Age      Flags
# ---------  --------------------  ---------  --------------  -------  -----
# req_abc123  iPhone-15-Pro         node       192.168.1.100   2m ago

# 2. 批准特定请求
openclaw devices approve req_abc123

# 3. 验证设备已配对
openclaw devices list
```

**自动化批准脚本**：

```bash
# 批准所有待处理请求（批量操作）
#!/bin/bash
pending=$(openclaw devices list --json | jq -r '.pending[].requestId')
for req in $pending; do
    echo "Approving $req..."
    openclaw devices approve "$req"
done
```

### 拒绝设备配对

```bash
# 拒绝指定的配对请求
openclaw devices reject <requestId>
```

### 管理已配对设备

```bash
# 移除已配对的设备
openclaw devices remove <deviceId>

# 清除所有配对设备（谨慎使用）
openclaw devices clear --yes

# 清除所有配对设备并拒绝所有待处理请求
openclaw devices clear --yes --pending
```

### 设备令牌管理

```bash
# 轮换设备令牌（更新指定角色的令牌）
openclaw devices rotate --device <deviceId> --role <role> [--scope <scope...>]

# 撤销设备令牌
openclaw devices revoke --device <deviceId> --role <role>
```

### 开发者模式下的设备配对

在开发者模式下（`pnpm dev`），使用 `--profile` 访问正式环境的设备列表：

```bash
# 查看正式环境的设备列表
pnpm dev devices list --profile default

# 批准正式环境的最新配对请求
pnpm dev devices approve --profile default

# 完整流程：查看并批准最新请求
pnpm dev devices list --profile default && pnpm dev devices approve --profile default

# 使用环境变量指定端口（如果网关非默认端口）
export OPENCLAW_GATEWAY_URL=ws://127.0.0.1:19001
pnpm dev devices list
pnpm dev devices approve
```

**如果网关使用了非默认端口（如 19001）**：

```bash
# 方法1：配置网关默认 URL（推荐，无需每次指定）
openclaw config set gateway.remote.url ws://127.0.0.1:19001
# 然后正常使用
pnpm dev devices list --profile default

# 方法2：使用环境变量（仅当前终端会话）
export OPENCLAW_GATEWAY_URL=ws://127.0.0.1:19001
pnpm dev devices list
```

**注意**：当使用 `--url` 参数时，出于安全考虑，必须同时提供 `--token` 或 `--password`：

```bash
# 错误：使用 --url 但不提供认证
pnpm dev devices list --url ws://127.0.0.1:19001

# 正确：使用 --url 并提供 token
pnpm dev devices list --url ws://127.0.0.1:19001 --token <your-gateway-token>

pnpm dev devices list --url ws://127.0.0.1:19001  --token fe4a818e2b9815678166ad18f2dcb604f7673e6ab6e027f6

# 正确：使用 --url 并提供密码
pnpm dev devices list --url ws://127.0.0.1:19001 --password <your-password>
```

### 远程网关的设备配对

如果网关运行在其他主机或 Docker 中，指定 URL 和认证信息：

```bash
# 使用 token 认证
openclaw devices list --url ws://gateway-host:18789 --token <token>
openclaw devices approve --url ws://gateway-host:18789 --token <token>

# 使用密码认证
openclaw devices approve --url ws://gateway-host:18789 --password <password>

# 指定超时时间（默认 10 秒）
openclaw devices list --url ws://gateway-host:18789 --token <token> --timeout 30000
openclaw devices approve --url ws://gateway-host:18789 --token <token> --timeout 30000
```

## 设备配对场景速查

| 场景 | 命令 |
|------|------|
| 查看待批准设备 | `openclaw devices list` |
| 批准最新请求 | `openclaw devices approve` |
| 批准指定请求 | `openclaw devices approve <requestId>` |
| 拒绝请求 | `openclaw devices reject <requestId>` |
| 移除已配对设备 | `openclaw devices remove <deviceId>` |
| 清除所有配对 | `openclaw devices clear --yes` |
| 开发模式查看设备 | `pnpm dev devices list --profile default` |
| 开发模式批准设备 | `pnpm dev devices approve --profile default` |
| 指定端口查看 | `pnpm dev devices list --url ws://127.0.0.1:19001 --token <token>` |
| 远程网关批准 | `openclaw devices approve --url ws://host:18789 --token <token>` |

## 设备配对故障排除

### 常见问题

**问题 1：没有待处理的配对请求**

```bash
# 检查设备列表
openclaw devices list
# 输出：No device pairing entries.

# 解决：确保设备已发起配对请求（在手机上扫描二维码或输入配对码）
```

**问题 2：网关连接失败**

```bash
# 错误：Failed to start CLI: Error: gateway url override requires explicit credentials
# 原因：使用了 --url 但没有提供 --token 或 --password

# 解决：提供认证信息
openclaw devices list --url ws://127.0.0.1:19001 --token <token>

# 或配置默认远程 URL（无需 --url）
openclaw config set gateway.remote.url ws://127.0.0.1:19001
openclaw devices list
```

**问题 3：批准失败，提示 requestId 不存在**

```bash
# 错误：unknown requestId
# 原因：请求 ID 已过期或已处理

# 解决：重新列出待处理请求获取最新的 requestId
openclaw devices list
openclaw devices approve <新的 requestId>
```

**问题 4：设备已配对但无法连接**

```bash
# 1. 检查设备是否在配对列表中
openclaw devices list

# 2. 如果设备存在但有问题，尝试移除后重新配对
openclaw devices remove <deviceId>
# 然后让设备重新发起配对请求并批准

# 3. 或者轮换设备令牌
openclaw devices rotate --device <deviceId> --role node
```

### 快速诊断命令

```bash
# 检查网关状态
openclaw status --deep

# 检查网关健康
openclaw health

# 查看详细设备信息（JSON 格式）
openclaw devices list --json | jq '.pending, .paired'
```

## 网关健康状态

如果 UI 显示离线状态，确保网关正在运行：

```bash
# 启动网关
pnpm gateway:dev

# 或者使用完整 CLI
pnpm dev gateway
```

网关默认运行在 `http://127.0.0.1:18789`，UI 将自动连接。
