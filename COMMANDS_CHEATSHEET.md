# OpenClaw 命令速查表

本文档整理了 OpenClaw 项目中所有可直接复制的命令，方便快速使用。

## 核心 CLI 命令

### 基础命令

```bash
# 初始化设置
openclaw setup

# 交互式入门向导
openclaw onboard

# 交互式配置向导
openclaw configure

# 健康检查和快速修复
openclaw doctor

# 打开控制面板
openclaw dashboard

# 重置本地配置
openclaw reset

# 卸载网关服务
openclaw uninstall
```

### 消息管理

```bash
# 发送消息
openclaw message send --channel telegram --to "@username" --text "Hello!"

# 管理消息
openclaw message --help

# 搜索和重新索引内存
openclaw memory search "关键词"
openclaw memory reindex
```

### 代理管理

```bash
# 运行代理对话
openclaw agent --message "你的消息"

# 管理代理
openclaw agents list
openclaw agents create --name "我的代理"
```

### 系统状态

```bash
# 查看通道状态
openclaw status

# 查看网关健康状态
openclaw health

# 列出会话
openclaw sessions list
```

### 浏览器管理

```bash
# 管理专用浏览器
openclaw browser start
openclaw browser stop
openclaw browser status
```

## 子命令参考

### 网关管理

```bash
# 运行网关
openclaw gateway run

# 检查网关状态
openclaw gateway status

# 停止网关
openclaw gateway stop
```

### 模型管理

```bash
# 列出可用模型
openclaw models list

# 扫描模型
openclaw models scan

# 配置模型
openclaw models configure
```

### 节点管理

```bash
# 管理节点
openclaw nodes list
openclaw nodes pair

# 运行节点主机
openclaw node run
```

### 沙箱管理

```bash
# 管理沙箱容器
openclaw sandbox create
openclaw sandbox list
openclaw sandbox clean
```

### 通道管理

```bash
# 列出所有通道
openclaw channels list

# 添加通道
openclaw channels add telegram --token "YOUR_TOKEN"

# 移除通道
openclaw channels remove telegram
```

### 插件管理

```bash
# 列出插件
openclaw plugins list

# 安装插件
openclaw plugins install @openclaw/telegram

# 启用/禁用插件
openclaw plugins enable @openclaw/telegram
openclaw plugins disable @openclaw/telegram
```

### 更新管理

```bash
# 检查更新
openclaw update status

# 更新到最新版本
openclaw update

# 切换到开发通道
openclaw update --channel dev
```

## 开发者模式命令

### 启用开发者模式

```bash
# 使用开发者模式
openclaw --dev gateway

# 使用命名配置文件
openclaw --profile test status
```

### 开发者网关

```bash
# 开发模式运行网关
openclaw gateway --dev

# 重置开发环境
openclaw gateway --dev --reset

# 强制启动（杀死现有进程）
openclaw gateway --dev --force
```

### 开发工具

```bash
# 开发模式运行终端 UI
pnpm tui:dev

# 开发模式运行 Web UI
pnpm ui:dev

# 运行实时测试
OPENCLAW_LIVE_TEST=1 pnpm test:live
```

## 常用开发脚本

### 构建和开发

```bash
# 完整构建
pnpm build

# 开发模式运行
pnpm dev

# 运行网关开发模式
pnpm gateway:dev

# 重置并运行网关开发模式
pnpm gateway:dev:reset
```

### 代码质量

```bash
# 运行所有检查
pnpm check

# 格式化代码
pnpm format:fix

# 运行测试
pnpm test

# 测试覆盖率
pnpm test:coverage
```

### 移动应用开发

```bash
# Android 构建和运行
pnpm android:assemble
pnpm android:run

# iOS 构建和运行
pnpm ios:build
pnpm ios:run
```

### macOS 应用开发

```bash
# 打包 macOS 应用
pnpm mac:package

# 重启 macOS 应用
pnpm mac:restart
```

## 插件命令

### 语音呼叫插件

```bash
# 发起语音呼叫
openclaw voicecall start --to "+1234567890"

# 继续通话
openclaw voicecall continue --call-id "call_123"

# 结束通话
openclaw voicecall end --call-id "call_123"
```

### 电话控制插件

```bash
# 电话控制功能
openclaw phone dial --number "+1234567890"
openclaw phone hangup
```

### 设备配对插件

```bash
# 设备配对
openclaw pair device --name "我的设备"
openclaw pair list
```

### Line 插件

```bash
# Line 卡片命令
openclaw card send --to "user_id" --template "welcome"
```

### 语音命令插件

```bash
# 语音命令
openclaw voice speak --text "你好，世界"
```

## 实用命令组合

### 快速启动工作流

```bash
# 1. 初始化设置
openclaw setup

# 2. 配置通道
openclaw configure

# 3. 启动网关
openclaw gateway run

# 4. 检查状态
openclaw status

# 5. 开始聊天
openclaw agent --message "你好"
```

### 开发者工作流

```bash
# 1. 切换到开发者模式
openclaw --dev gateway

# 2. 构建项目
pnpm build

# 3. 运行测试
pnpm test

# 4. 开发模式运行
pnpm dev
```

### 故障排除工作流

```bash
# 1. 检查系统健康
openclaw doctor

# 2. 查看网关日志
openclaw logs

# 3. 重置配置（如果需要）
openclaw reset

# 4. 重新启动
openclaw gateway run
```

## 环境变量参考

### 常用环境变量

```bash
# 启用开发者模式
export OPENCLAW_PROFILE=dev

# 设置状态目录
export OPENCLAW_STATE_DIR=~/.openclaw-dev

# 设置网关端口
export OPENCLAW_GATEWAY_PORT=19001

# 启用实时测试
export OPENCLAW_LIVE_TEST=1
```

### 配置环境变量

```bash
# 一次性设置
OPENCLAW_LIVE_TEST=1 pnpm test:live

# 永久设置（添加到 ~/.bashrc 或 ~/.zshrc）
echo 'export OPENCLAW_PROFILE=dev' >> ~/.zshrc
source ~/.zshrc
```

## 命令帮助和文档

### 获取帮助

```bash
# 查看所有命令
openclaw --help

# 查看特定命令帮助
openclaw gateway --help
openclaw message --help

# 查看子命令帮助
openclaw plugins list --help
```

### 文档搜索

```bash
# 搜索文档
openclaw docs search "网关配置"

# 打开特定文档
openclaw docs open configuration
```

## 注意事项

1. **命令前缀**：所有 CLI 命令以 `openclaw` 开头
2. **开发脚本**：使用 `pnpm run <script>` 执行
3. **开发者模式**：`--dev` 和 `--profile` 不能同时使用
4. **权限要求**：某些命令可能需要管理员权限
5. **网络要求**：部分命令需要互联网连接

## 快速参考卡片

### 日常使用

```
openclaw status          # 检查状态
openclaw agent           # 开始聊天
openclaw dashboard       # 打开控制面板
openclaw doctor          # 健康检查
```

### 开发调试

```
pnpm build              # 构建项目
pnpm test               # 运行测试
pnpm dev                # 开发模式
openclaw --dev gateway  # 开发者网关
```

### 系统管理

```
openclaw gateway run    # 启动网关
openclaw reset          # 重置配置
openclaw update         # 更新软件
openclaw plugins list   # 管理插件
```

---

_基于 COMMANDS.md 整理，最后更新：2026年3月5日_
_原始文档：COMMANDS.md_
_GitHub 仓库：https://github.com/openclaw/openclaw_
_官方文档：https://docs.openclaw.ai/_
