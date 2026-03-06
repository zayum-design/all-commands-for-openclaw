# OpenClaw 快速命令参考

## 🚀 快速开始

### 首次使用

```bash
# 1. 初始化设置
openclaw setup

# 2. 配置通道和模型
openclaw configure

# 3. 启动网关
openclaw gateway run

# 4. 检查状态
openclaw status

# 5. 开始聊天
openclaw agent --message "你好"
```

### 日常使用

```bash
# 检查系统状态
openclaw status
openclaw doctor

# 打开控制面板
openclaw dashboard

# 发送消息
openclaw message send --channel telegram --to "@username" --text "消息内容"

# 管理会话
openclaw sessions list
```

## 📋 核心命令速查

### 系统管理

```bash
openclaw setup          # 初始化设置
openclaw onboard        # 交互式入门向导
openclaw configure      # 配置向导
openclaw doctor         # 健康检查
openclaw reset          # 重置配置
openclaw uninstall      # 卸载服务
```

### 网关管理

```bash
openclaw gateway run    # 启动网关
openclaw gateway stop   # 停止网关
openclaw gateway status # 查看状态
openclaw health         # 健康检查
```

### 消息和代理

```bash
openclaw agent --message "消息"      # 运行代理对话
openclaw message send --channel <通道> --to <目标> --text "消息"
openclaw memory search "关键词"      # 搜索记忆
openclaw memory reindex             # 重新索引
```

### 通道管理

```bash
openclaw channels list              # 列出通道
openclaw channels add telegram --token "TOKEN"  # 添加通道
openclaw channels remove telegram   # 移除通道
```

### 插件管理

```bash
openclaw plugins list              # 列出插件
openclaw plugins install @openclaw/telegram  # 安装插件
openclaw plugins enable @openclaw/telegram   # 启用插件
openclaw plugins disable @openclaw/telegram  # 禁用插件
```

## 🔧 开发者命令

### 开发者模式

```bash
# 启用开发者模式
openclaw --dev gateway
openclaw --profile test status

# 开发者网关
openclaw gateway --dev
openclaw gateway --dev --reset
openclaw gateway --dev --force
```

### 开发脚本

```bash
# 构建和运行
pnpm build
pnpm dev
pnpm test

# 代码质量
pnpm check
pnpm format:fix
pnpm test:coverage

# 移动应用
pnpm android:run
pnpm ios:run
pnpm mac:package
```

### 实时测试

```bash
OPENCLAW_LIVE_TEST=1 pnpm test:live
pnpm test:docker:live-gateway
pnpm test:docker:live-models
```

## 🔌 插件命令

### 语音呼叫

```bash
openclaw voicecall start --to "+1234567890"
openclaw voicecall continue --call-id "call_123"
openclaw voicecall end --call-id "call_123"
```

### 电话控制

```bash
openclaw phone dial --number "+1234567890"
openclaw phone hangup
```

### 设备配对

```bash
openclaw pair device --name "设备名"
openclaw pair list
```

## ⚙️ 实用工作流

### 快速启动

```bash
# 1. 设置
openclaw setup
# 2. 配置
openclaw configure
# 3. 启动
openclaw gateway run
# 4. 验证
openclaw status
# 5. 使用
openclaw agent --message "开始工作"
```

### 故障排除

```bash
# 1. 检查
openclaw doctor
# 2. 查看日志
openclaw logs
# 3. 重置（如果需要）
openclaw reset
# 4. 重启
openclaw gateway run
```

### 更新管理

```bash
openclaw update status      # 检查更新
openclaw update             # 更新到最新
openclaw update --channel dev  # 切换到开发通道
```

## 📝 环境变量

### 常用变量

```bash
export OPENCLAW_PROFILE=dev
export OPENCLAW_STATE_DIR=~/.openclaw-dev
export OPENCLAW_GATEWAY_PORT=19001
export OPENCLAW_LIVE_TEST=1
```

### 临时使用

```bash
OPENCLAW_LIVE_TEST=1 pnpm test:live
```

## ❓ 帮助和文档

```bash
openclaw --help                    # 所有命令
openclaw <command> --help          # 特定命令
openclaw docs search "关键词"      # 搜索文档
openclaw docs open configuration   # 打开文档
```

## 🎯 常用命令组合

### 日常监控

```bash
openclaw status && openclaw health && openclaw doctor
```

### 开发测试循环

```bash
pnpm build && pnpm test && pnpm dev
```

### 插件管理流程

```bash
openclaw plugins list
openclaw plugins install @openclaw/telegram
openclaw plugins enable @openclaw/telegram
openclaw channels add telegram --token "YOUR_TOKEN"
```

## ⚠️ 注意事项

1. **命令格式**：所有 CLI 命令以 `openclaw` 开头
2. **包管理器**：开发脚本使用 `pnpm run <script>` 或 `pnpm <script>`
3. **模式冲突**：`--dev` 和 `--profile` 不能同时使用
4. **权限**：某些命令可能需要管理员权限
5. **网络**：部分功能需要互联网连接

---

**快速参考**：将此文件保存在方便的位置，或打印出来作为桌面参考。

_基于 COMMANDS.md 整理 | 最后更新：2026年3月5日_
_GitHub: https://github.com/openclaw/openclaw_
_文档: https://docs.openclaw.ai/_
