# all-commands-for-openclaw
All available commands for OpenCLAW


# OpenClaw 命令参考

本文档列出了 OpenClaw 项目中所有可用的命令，包括 CLI 命令和开发脚本。

## CLI 命令

OpenClaw 提供了丰富的 CLI 命令，可以通过 `openclaw <command>` 执行。

### 核心命令

| 命令 | 描述 | 是否有子命令 |
|------|------|--------------|
| `setup` | 初始化本地配置和代理工作空间 | 否 |
| `onboard` | 网关、工作空间和技能的交互式入门向导 | 否 |
| `configure` | 凭证、通道、网关和代理默认值的交互式设置向导 | 否 |
| `config` | 非交互式配置助手（获取/设置/取消设置/文件/验证）。默认：启动设置向导。 | 是 |
| `doctor` | 网关和通道的健康检查 + 快速修复 | 否 |
| `dashboard` | 使用当前令牌打开控制 UI | 否 |
| `reset` | 重置本地配置/状态（保留 CLI 安装） | 否 |
| `uninstall` | 卸载网关服务 + 本地数据（CLI 保留） | 否 |
| `message` | 发送、读取和管理消息 | 是 |
| `memory` | 搜索和重新索引内存文件 | 是 |
| `agent` | 通过网关运行一个代理回合 | 否 |
| `agents` | 管理隔离的代理（工作空间、身份验证、路由） | 是 |
| `status` | 显示通道健康和最近会话接收者 | 否 |
| `health` | 从运行的网关获取健康状态 | 否 |
| `sessions` | 列出存储的对话会话 | 是 |
| `browser` | 管理 OpenClaw 的专用浏览器（Chrome/Chromium） | 是 |

### 子 CLI 命令

| 命令 | 描述 | 是否有子命令 |
|------|------|--------------|
| `acp` | 代理控制协议工具 | 是 |
| `gateway` | 运行、检查和查询 WebSocket 网关 | 是 |
| `daemon` | 网关服务（旧别名） | 是 |
| `logs` | 通过 RPC 跟踪网关文件日志 | 否 |
| `system` | 系统事件、心跳和存在状态 | 是 |
| `models` | 发现、扫描和配置模型 | 是 |
| `approvals` | 管理执行批准（网关或节点主机） | 是 |
| `nodes` | 管理网关拥有的节点配对和节点命令 | 是 |
| `devices` | 设备配对 + 令牌管理 | 是 |
| `node` | 运行和管理无头节点主机服务 | 是 |
| `sandbox` | 管理代理隔离的沙箱容器 | 是 |
| `tui` | 打开连接到网关的终端 UI | 否 |
| `cron` | 通过网关调度器管理 cron 作业 | 是 |
| `dns` | 广域发现的 DNS 助手（Tailscale + CoreDNS） | 是 |
| `docs` | 搜索实时 OpenClaw 文档 | 否 |
| `hooks` | 管理内部代理钩子 | 是 |
| `webhooks` | Webhook 助手和集成 | 是 |
| `qr` | 生成 iOS 配对 QR/设置代码 | 否 |
| `clawbot` | 旧版 clawbot 命令别名 | 是 |
| `pairing` | 安全 DM 配对（批准入站请求） | 是 |
| `plugins` | 管理 OpenClaw 插件和扩展 | 是 |
| `channels` | 管理连接的聊天通道（Telegram、Discord 等） | 是 |
| `directory` | 查找支持的聊天通道的联系人和群组 ID（自己、对等方、群组） | 是 |
| `security` | 安全工具和本地配置审计 | 是 |
| `secrets` | 密钥运行时重新加载控制 | 是 |
| `skills` | 列出和检查可用技能 | 是 |
| `update` | 更新 OpenClaw 并检查更新通道状态 | 是 |
| `completion` | 生成 shell 补全脚本 | 否 |

### 插件命令

以下命令由插件提供，仅在相应插件安装并启用时可用：

| 命令 | 描述 | 插件 | 是否有子命令 |
|------|------|------|--------------|
| `voicecall` | 语音呼叫功能（发起、继续、结束通话等） | voice-call | 是 |
| `phone` | 电话控制功能 | phone-control | 是 |
| `pair` | 设备配对功能 | device-pair | 是 |
| `card` | Line 卡片命令 | line | 否 |
| `voice` | 语音命令 | talk-voice | 否 |

## 开发者模式命令

OpenClaw 提供了专门的开发者模式，用于隔离开发环境、测试新功能和调试。开发者模式会创建独立的状态目录、配置和工作空间，避免影响生产环境。

### 全局开发者模式选项

| 选项 | 描述 | 示例 |
|------|------|------|
| `--dev` | 启用开发者模式：状态目录隔离在 `~/.openclaw-dev`，默认网关端口 19001，并偏移派生端口（浏览器/画布） | `openclaw --dev gateway` |
| `--profile <name>` | 使用命名配置文件（将 OPENCLAW_STATE_DIR/OPENCLAW_CONFIG_PATH 隔离在 `~/.openclaw-<name>` 下） | `openclaw --profile test status` |

**注意**：`--dev` 和 `--profile` 不能同时使用。

### 开发者模式专用命令

#### 1. 网关开发者模式
| 命令 | 描述 | 选项 | 示例 |
|------|------|------|------|
| `openclaw gateway --dev` | 在开发者模式下运行网关（隔离状态/配置在 ws://127.0.0.1:19001） | `--dev`: 创建开发配置+工作空间（无 BOOTSTRAP.md）<br>`--reset`: 重置开发配置+凭证+会话+工作空间（需要 `--dev`）<br>`--force`: 启动前杀死目标端口上的现有监听器 | `openclaw --dev gateway`<br>`openclaw gateway --dev --reset`<br>`openclaw gateway --dev --force` |
| `openclaw gateway:dev` | 开发模式运行网关（npm script） | 无 | `pnpm gateway:dev` |
| `openclaw gateway:dev:reset` | 开发模式运行网关并重置（npm script） | 无 | `pnpm gateway:dev:reset` |

#### 2. 更新通道管理
| 命令 | 描述 | 选项 | 示例 |
|------|------|------|------|
| `openclaw update --channel dev` | 切换到开发更新通道（git + npm） | `--channel dev`: 切换到开发通道<br>`--tag <dist-tag|version>`: 覆盖 npm dist-tag 或版本<br>`--dry-run`: 预览操作而不更改任何内容 | `openclaw update --channel dev`<br>`openclaw update --tag beta`<br>`openclaw update --dry-run` |
| `openclaw update status` | 显示更新通道和版本状态 | `--json`: JSON 输出<br>`--timeout <seconds>`: 更新检查超时时间 | `openclaw update status`<br>`openclaw update status --json` |
| `openclaw update wizard` | 交互式更新向导 | `--timeout <seconds>`: 每个更新步骤的超时时间 | `openclaw update wizard` |

#### 3. 开发工具和测试
| 命令 | 描述 | 选项 | 示例 |
|------|------|------|------|
| `openclaw tui:dev` | 开发模式运行终端 UI（npm script） | 无 | `pnpm tui:dev` |
| `openclaw ui:dev` | 开发模式运行 UI（npm script） | 无 | `pnpm ui:dev` |
| `openclaw test:live` | 运行实时测试（需要真实密钥） | `OPENCLAW_LIVE_TEST=1`: 启用实时测试 | `OPENCLAW_LIVE_TEST=1 pnpm test:live` |
| `openclaw test:docker:live-gateway` | 运行 Docker 实时网关测试 | 无 | `pnpm test:docker:live-gateway` |
| `openclaw test:docker:live-models` | 运行 Docker 实时模型测试 | 无 | `pnpm test:docker:live-models` |

### 开发者模式环境变量

| 环境变量 | 描述 | 默认值（开发者模式） |
|----------|------|---------------------|
| `OPENCLAW_PROFILE` | 配置文件名称 | `dev` |
| `OPENCLAW_STATE_DIR` | 状态目录路径 | `~/.openclaw-dev` |
| `OPENCLAW_CONFIG_PATH` | 配置文件路径 | `~/.openclaw-dev/openclaw.json` |
| `OPENCLAW_GATEWAY_PORT` | 网关端口 | `19001` |
| `OPENCLAW_LIVE_TEST` | 启用实时测试 | `1`（当设置时） |

### 开发者工作空间

开发者模式会创建独立的工作空间，包含以下文件：
- `AGENTS.md` - OpenClaw 开发工作空间配置
- `SOUL.md` - 开发角色定义
- `TOOLS.md` - 用户工具笔记
- `IDENTITY.md` - 代理身份配置
- `USER.md` - 用户配置文件

## 开发脚本（npm scripts）

这些命令可以通过 `pnpm run <script>` 或 `npm run <script>` 执行。

### 构建和开发

| 脚本 | 描述 |
|------|------|
| `build` | 完整构建项目（包括 A2UI 捆绑、TypeScript 编译、插件 SDK 生成等） |
| `build:plugin-sdk:dts` | 构建插件 SDK 类型定义 |
| `build:strict-smoke` | 严格构建（用于冒烟测试） |
| `canvas:a2ui:bundle` | 捆绑 A2UI 资源 |
| `dev` | 开发模式运行 |
| `gateway:dev` | 开发模式运行网关 |
| `gateway:dev:reset` | 开发模式运行网关并重置 |
| `gateway:watch` | 监视模式运行网关 |
| `openclaw` | 运行 OpenClaw CLI |
| `openclaw:rpc` | 以 RPC 模式运行代理 |
| `start` | 启动项目 |
| `tui` | 运行终端 UI |
| `tui:dev` | 开发模式运行终端 UI |
| `ui:build` | 构建 UI |
| `ui:dev` | 开发模式运行 UI |
| `ui:install` | 安装 UI 依赖 |

### 代码质量和测试

| 脚本 | 描述 |
|------|------|
| `check` | 运行所有代码质量检查（格式化、类型检查、lint 等） |
| `check:docs` | 检查文档 |
| `check:host-env-policy:swift` | 检查 Swift 主机环境策略 |
| `check:loc` | 检查 TypeScript 文件行数 |
| `deadcode:ci` | CI 死代码报告 |
| `deadcode:knip` | 使用 Knip 检查死代码 |
| `deadcode:report` | 死代码报告 |
| `deadcode:report:ci:knip` | CI Knip 死代码报告 |
| `deadcode:report:ci:ts-prune` | CI ts-prune 死代码报告 |
| `deadcode:report:ci:ts-unused` | CI ts-unused-exports 死代码报告 |
| `deadcode:ts-prune` | 使用 ts-prune 检查死代码 |
| `deadcode:ts-unused` | 使用 ts-unused-exports 检查未使用的导出 |
| `format` | 格式化代码 |
| `format:all` | 格式化所有代码（包括 Swift） |
| `format:check` | 检查代码格式化 |
| `format:diff` | 格式化并显示差异 |
| `format:docs` | 格式化文档 |
| `format:docs:check` | 检查文档格式化 |
| `format:fix` | 修复代码格式化 |
| `format:swift` | 格式化 Swift 代码 |
| `lint` | 运行 linter |
| `lint:agent:ingress-owner` | 检查代理入口所有者上下文 |
| `lint:all` | 运行所有 lint 检查 |
| `lint:auth:no-pairing-store-group` | 检查无配对存储组身份验证 |
| `lint:auth:pairing-account-scope` | 检查配对账户范围 |
| `lint:docs` | lint 文档 |
| `lint:docs:fix` | 修复文档 lint 问题 |
| `lint:fix` | 修复 lint 问题 |
| `lint:plugins:no-register-http-handler` | 检查插件无注册 HTTP 处理程序 |
| `lint:swift` | lint Swift 代码 |
| `lint:tmp:channel-agnostic-boundaries` | 检查通道无关边界 |
| `lint:tmp:no-random-messaging` | 检查无随机消息 |
| `lint:tmp:no-raw-channel-fetch` | 检查无原始通道获取 |
| `lint:ui:no-raw-window-open` | 检查 UI 无原始窗口打开 |
| `lint:webhook:no-low-level-body-read` | 检查 Webhook 无低级正文读取 |
| `test` | 运行测试 |
| `test:all` | 运行所有测试 |
| `test:channels` | 运行通道测试 |
| `test:coverage` | 运行测试并生成覆盖率报告 |
| `test:docker:all` | 运行所有 Docker 测试 |
| `test:docker:cleanup` | 清理 Docker 测试 |
| `test:docker:doctor-switch` | 运行 Docker 医生切换测试 |
| `test:docker:gateway-network` | 运行 Docker 网关网络测试 |
| `test:docker:live-gateway` | 运行 Docker 实时网关测试 |
| `test:docker:live-models` | 运行 Docker 实时模型测试 |
| `test:docker:onboard` | 运行 Docker 入门测试 |
| `test:docker:plugins` | 运行 Docker 插件测试 |
| `test:docker:qr` | 运行 Docker QR 测试 |
| `test:e2e` | 运行端到端测试 |
| `test:extensions` | 运行扩展测试 |
| `test:fast` | 快速运行测试 |
| `test:force` | 强制运行测试 |
| `test:gateway` | 运行网关测试 |
| `test:install:e2e` | 运行安装端到端测试 |
| `test:install:e2e:anthropic` | 运行 Anthropic 安装端到端测试 |
| `test:install:e2e:openai` | 运行 OpenAI 安装端到端测试 |
| `test:install:smoke` | 运行安装冒烟测试 |
| `test:live` | 运行实时测试 |
| `test:macmini` | 在 Mac mini 上运行测试 |
| `test:perf:budget` | 运行性能预算测试 |
| `test:perf:hotspots` | 运行性能热点测试 |
| `test:sectriage` | 运行安全分类测试 |
| `test:ui` | 运行 UI 测试 |
| `test:voicecall:closedloop` | 运行语音呼叫闭环测试 |
| `test:watch` | 监视模式运行测试 |

### 移动应用开发

| 脚本 | 描述 |
|------|------|
| `android:assemble` | 构建 Android 应用 |
| `android:format` | 格式化 Android 代码 |
| `android:install` | 安装 Android 应用 |
| `android:lint` | lint Android 代码 |
| `android:lint:android` | Android lint 检查 |
| `android:run` | 运行 Android 应用 |
| `android:test` | 运行 Android 测试 |
| `android:test:integration` | 运行 Android 集成测试 |
| `ios:build` | 构建 iOS 应用 |
| `ios:gen` | 生成 iOS 项目文件 |
| `ios:open` | 打开 iOS 项目 |
| `ios:run` | 运行 iOS 应用 |

### macOS 应用开发

| 脚本 | 描述 |
|------|------|
| `mac:open` | 打开 macOS 应用 |
| `mac:package` | 打包 macOS 应用 |
| `mac:restart` | 重启 macOS 应用 |

### 文档

| 脚本 | 描述 |
|------|------|
| `docs:bin` | 构建文档列表 |
| `docs:check-links` | 检查文档链接 |
| `docs:dev` | 开发模式运行文档 |
| `docs:list` | 列出文档 |
| `docs:spellcheck` | 拼写检查文档 |
| `docs:spellcheck:fix` | 修复文档拼写 |

### 协议和代码生成

| 脚本 | 描述 |
|------|------|
| `gen:host-env-policy:swift` | 生成 Swift 主机环境策略 |
| `ghsa:patch` | 应用 GHSA 补丁 |
| `plugins:sync` | 同步插件版本 |
| `protocol:check` | 检查协议生成 |
| `protocol:gen` | 生成协议 |
| `protocol:gen:swift` | 生成 Swift 协议 |
| `release:check` | 检查发布 |

### 其他工具

| 脚本 | 描述 |
|------|------|
| `moltbot:rpc` | 以 RPC 模式运行 Moltbot |
| `prepack` | 预打包构建 |
| `prepare` | 准备钩子 |

## 使用示例

### CLI 命令示例
```bash
# 初始化设置
openclaw setup

# 运行交互式入门向导
openclaw onboard

# 检查系统健康状态
openclaw doctor

# 发送消息
openclaw message send --channel telegram --to "@username" --text "Hello!"

# 查看通道状态
openclaw status

# 管理插件
openclaw plugins list

# 直接进入聊天（完成 onboarding 后）
openclaw agent --message "你好，开始聊天吧"

# 打开 Web 控制界面
openclaw dashboard

# 打开终端用户界面
openclaw tui
```

### 常见问题解答

#### 1. 我已经完成了 onboarding 设置，如何直接进入 chat？
`openclaw onboard` 是**一次性设置向导**，用于配置网关、通道、技能等。设置完成后就不需要再运行它。

完成设置后，可以使用以下命令直接进入 chat：

- **`openclaw agent`** - 主要的聊天命令，使用已配置的模型进行对话
  ```bash
  openclaw agent --message "你的消息"
  ```
  
- **`openclaw dashboard`** - 打开 Web 控制界面进行交互
  ```bash
  openclaw dashboard
  ```
  
- **`openclaw tui`** - 打开终端用户界面
  ```bash
  openclaw tui
  ```

#### 2. `openclaw agent` 和 `openclaw message send` 有什么区别？
- **`openclaw agent`**：用于对话式聊天，维护会话状态，支持流式响应
- **`openclaw message send`**：用于直接向特定通道发送消息，更适合一次性通知

#### 3. 如果网关没有运行怎么办？
```bash
# 启动网关
openclaw gateway run

# 检查状态
openclaw status
openclaw health
```

### 开发者模式使用示例
```bash
# 在开发者模式下启动网关
openclaw --dev gateway

# 重置开发者环境并重新启动
openclaw gateway --dev --reset

# 切换到开发更新通道
openclaw update --channel dev

# 检查开发者模式状态
openclaw update status

# 运行开发者模式测试
OPENCLAW_LIVE_TEST=1 pnpm test:live

# 使用命名配置文件
openclaw --profile test status
```

### 开发脚本示例
```bash
# 构建项目
pnpm build

# 运行测试
pnpm test

# 格式化代码
pnpm format:fix

# 运行开发服务器
pnpm dev

# 构建 macOS 应用
pnpm mac:package
```

## 注意事项

1. **CLI 命令**：所有 CLI 命令都通过 `openclaw` 二进制文件执行
2. **开发脚本**：使用 `pnpm run <script>` 执行（推荐使用 pnpm）
3. **环境变量**：某些命令可能需要特定的环境变量，请参考相关文档
4. **子命令**：带有子命令的命令可以通过 `openclaw <command> --help` 查看可用子命令
5. **开发者模式**：使用 `--dev` 标志隔离开发环境，避免影响生产配置
6. **配置文件**：`--dev` 和 `--profile` 不能同时使用

## 相关文档

- [OpenClaw 官方文档](https://docs.openclaw.ai/)
- [GitHub 仓库](https://github.com/openclaw/openclaw)
- [贡献指南](CONTRIBUTING.md)
- [开发者模式文档](https://docs.openclaw.ai/development/dev-mode)

---

*最后更新：2026年3月5日*

