# OpenClaw 完整安装配置指南

## 系统环境
- **操作系统**: Linux (Ubuntu/CentOS/Debian)
- **Node.js 版本**: v22.x (通过 NVM 安装)
- **OpenClaw 版本**: 2026.3.2
- **进程管理**: PM2
- **Web 访问**: 宝塔面板反向代理

---

## 一、安装 Node.js 和 OpenClaw

### 1.1 安装 NVM (Node Version Manager)

```bash
# 下载并安装 NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# 加载 NVM 环境变量
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# 验证安装
nvm --version
```

### 1.2 安装 Node.js 22

```bash
# 安装 Node.js 22
nvm install 22
nvm use 22
nvm alias default 22

# 验证版本
node --version  # 应显示 v22.x.x
npm --version
```

### 1.3 安装 OpenClaw

```bash
# 全局安装 OpenClaw
npm install -g openclaw

# 验证安装
openclaw --version  # 应显示 2026.3.2
```

---

## 二、配置 OpenClaw

### 2.1 创建配置文件

```bash
# 创建配置目录
mkdir -p ~/.openclaw
mkdir -p /root/openclaw-workspace

# 创建配置文件
cat > ~/.openclaw/openclaw.json << 'EOF'
{
  "tools": {
    "profile": "coding",
    "allow": ["group:fs", "group:runtime", "group:sessions"]
  },
  "agent": {
    "workspace": "/root/openclaw-workspace",
    "skipBootstrap": false
  },
  "gateway": {
    "port": 18789,
    "bind": "local",
    "controlUi": {
      "enabled": true
    },
    "auth": {
      "mode": "token"
    }
  }
}
EOF
```

### 2.2 配置说明

| 配置项 | 说明 |
|--------|------|
| `tools.allow` | 启用的工具组，`group:fs` 为文件系统工具 |
| `agent.workspace` | 默认工作目录，AI 可读写此目录 |
| `gateway.port` | 网关端口 |
| `gateway.bind` | 绑定模式，`local` 仅本地访问（安全） |
| `gateway.auth.mode` | 认证模式，`token` 为令牌认证 |

---

## 三、使用 PM2 管理 OpenClaw 进程

### 3.1 安装 PM2

```bash
# 安装 PM2
npm install -g pm2
```

### 3.2 启动 OpenClaw 网关

```bash
# 使用 PM2 启动 OpenClaw
pm2 start "openclaw gateway run" --name openclaw-gateway

# 查看状态
pm2 status

# 查看日志
pm2 logs openclaw-gateway
```

### 3.3 PM2 常用命令

| 命令 | 说明 |
|------|------|
| `pm2 restart openclaw-gateway` | 重启网关 |
| `pm2 stop openclaw-gateway` | 停止网关 |
| `pm2 delete openclaw-gateway` | 删除进程 |
| `pm2 logs openclaw-gateway` | 查看实时日志 |
| `pm2 logs --lines 100` | 查看最后 100 行日志 |
| `pm2 show openclaw-gateway` | 查看详细信息 |

### 3.4 设置开机自启

```bash
# 保存当前进程列表
pm2 save

# 生成开机启动脚本
pm2 startup

# 按提示执行生成的命令（示例）
sudo env PATH=$PATH:/root/.nvm/versions/node/v22.22.1/bin /root/.nvm/versions/node/v22.22.1/lib/node_modules/pm2/bin/pm2 startup systemd -u root --hp /root
```

---

## 四、宝塔面板配置 Web 访问

### 4.1 开放端口

1. 登录宝塔面板
2. 点击 **安全** → **添加端口规则**
3. 端口：`18789`
4. 备注：`OpenClaw`

### 4.2 创建反向代理站点

1. **网站** → **添加站点**
2. 输入域名（如 `openclaw.yourdomain.com`）
3. 选择 **纯静态**
4. 点击 **设置** → **反向代理**
5. 添加反向代理：
   - **代理名称**：`openclaw`
   - **目标 URL**：`http://127.0.0.1:18789`
   - **发送 Host 头**：勾选 ✓
6. 保存

### 4.3 配置 HTTPS（推荐）

1. 站点设置 → **SSL** → **Let's Encrypt**
2. 申请证书
3. 开启 **强制 HTTPS**

### 4.4 访问 Web UI

```
https://your-domain.com/?token=你的Token
```

获取 Token：
```bash
openclaw config get gateway.auth.token
```

---

## 五、SSH 隧道访问（备选方案）

如果不使用域名，可通过 SSH 隧道本地访问：

```bash
# 在本地电脑执行
ssh -N -L 18789:127.0.0.1:18789 root@你的服务器IP

# 然后浏览器访问
http://localhost:18789
```

---

## 六、验证安装

### 6.1 检查服务状态

```bash
# 检查进程
pm2 status

# 检查端口
ss -tlnp | grep 18789

# 本地测试
curl http://127.0.0.1:18789
```

### 6.2 测试文件操作

在 OpenClaw Web 界面中对话测试：

```
请创建文件 /root/openclaw-workspace/test.txt，内容为 "Hello OpenClaw!"
```

成功后会看到：
- 文件创建在 `/root/openclaw-workspace/test.txt`
- 日志显示 `[tool: write]` 操作

---

## 七、故障排查

### 7.1 端口被占用

```bash
# 查找占用 18789 的进程
lsof -i:18789

# 结束进程
kill -9 <PID>
```

### 7.2 权限不足

```bash
# 确保工作目录权限正确
chmod 755 /root/openclaw-workspace
```

### 7.3 环境变量丢失

如果重启后 `openclaw` 命令找不到：

```bash
# 重新加载环境
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm use 22
```

### 7.4 查看详细日志

```bash
# PM2 日志
pm2 logs openclaw-gateway --lines 200

# 系统日志
journalctl -u pm2-root -f
```

---

## 八、目录结构

```
/root/
├── .nvm/                          # NVM 安装目录
│   └── versions/node/v22.22.1/    # Node.js 22
├── .openclaw/                     # OpenClaw 配置
│   ├── openclaw.json              # 主配置文件
│   └── workspace/                 # 默认工作空间
├── openclaw-workspace/            # AI 可读写的工作目录
└── .pm2/                          # PM2 进程管理文件
```

---

## 九、安全建议

1. **使用 Local 绑定模式**：`"bind": "local"` 仅允许本地访问，通过反向代理暴露
2. **启用 Token 认证**：防止未授权访问
3. **限制工作目录**：AI 只能访问 `/root/openclaw-workspace`，无法操作系统关键文件
4. **使用 HTTPS**：生产环境必须配置 SSL 证书
5. **定期备份**：重要工作目录定期备份

---

## 十、快速启动脚本

保存为 `start-openclaw.sh`：

```bash
#!/bin/bash
# OpenClaw 快速启动脚本

# 加载环境
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm use 22

# 创建工作目录
mkdir -p /root/openclaw-workspace

# 启动或重启
if pm2 list | grep -q "openclaw-gateway"; then
    echo "重启 OpenClaw..."
    pm2 restart openclaw-gateway
else
    echo "启动 OpenClaw..."
    pm2 start "openclaw gateway run" --name openclaw-gateway
fi

# 显示状态
pm2 status

echo "OpenClaw 已启动，访问: http://服务器IP:18789"
echo "Token: $(openclaw config get gateway.auth.token 2>/dev/null || echo '请查看配置文件')"
```

赋予执行权限：
```bash
chmod +x start-openclaw.sh
./start-openclaw.sh
```

---

**文档版本**: 2026.3.2  
**最后更新**: 2026-03-06
