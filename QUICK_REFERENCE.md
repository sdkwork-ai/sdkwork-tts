# SDKWork-TTS 快速参考卡片

## 🚀 快速开始

### 安装
```bash
# Linux/macOS
curl -fsSL https://github.com/sdkwork-ai/sdkwork-tts/raw/main/scripts/install.sh | bash

# Windows PowerShell
Invoke-WebRequest -Uri "https://github.com/sdkwork-ai/sdkwork-tts/raw/main/scripts/install.ps1" -OutFile install.ps1
.\install.ps1
```

### 验证
```bash
# Linux/macOS
~/.sdkwork-tts/bin/verify_install.sh

# Windows
.\scripts\verify_install.ps1
```

### 启动
```bash
# Local 模式
sdkwork-tts server --mode local

# Cloud 模式
sdkwork-tts server --mode cloud

# 配置文件
sdkwork-tts server --config server.yaml
```

---

## 🐳 Docker

### 启动
```bash
# CPU
docker compose --profile cpu up -d

# GPU
docker compose --profile gpu up -d

# 单个容器
docker run -d -p 8080:8080 --name sdkwork-tts sdkwork-tts:latest-cpu
```

### 管理
```bash
# 日志
docker logs -f sdkwork-tts

# 停止
docker stop sdkwork-tts && docker rm sdkwork-tts

# 重启
docker restart sdkwork-tts
```

---

## 🔧 Makefile

### 构建
```bash
make build          # Release 构建
make build-gpu      # GPU 构建
make build-debug    # Debug 构建
```

### 测试
```bash
make test           # 运行测试
make test-all       # 完整测试
make check          # Clippy + 格式检查
```

### 运行
```bash
make run            # Local 模式
make run-cloud      # Cloud 模式
make run-hybrid     # 混合模式
make dev            # 开发模式 (热重载)
```

### Docker
```bash
make docker         # 构建镜像
make docker-run     # 运行容器
make docker-logs    # 查看日志
make docker-stop    # 停止容器
```

### 工具
```bash
make help           # 显示帮助
make clean          # 清理
make install        # 安装
make uninstall      # 卸载
make version        # 版本信息
make fmt            # 格式化
make fix            # 自动修复
make doc            # 生成文档
```

---

## 📡 API 端点

### 健康检查
```bash
curl http://localhost:8080/health
```

### 服务器统计
```bash
curl http://localhost:8080/api/v1/stats
```

### 语音合成
```bash
curl -X POST http://localhost:8080/api/v1/synthesis \
  -H "Content-Type: application/json" \
  -d '{
    "text": "你好，世界",
    "speaker": "vivian",
    "channel": "local"
  }'
```

### 列出 Speaker
```bash
curl http://localhost:8080/api/v1/speakers
```

### 语音设计
```bash
curl -X POST http://localhost:8080/api/v1/voice/design \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello",
    "voice_design": {"description": "A warm female voice"}
  }'
```

### 语音克隆
```bash
curl -X POST http://localhost:8080/api/v1/voice/clone \
  -H "Content-Type: application/json" \
  -d '{
    "text": "克隆声音",
    "voice_clone": {"reference_audio": "ref.wav"}
  }'
```

---

## 🔍 故障排查

### 诊断工具
```bash
./scripts/diagnose.sh
```

### 查看日志
```bash
# systemd
sudo journalctl -u sdkwork-tts -f

# Docker
docker logs -f sdkwork-tts

# 文件日志
tail -f logs/server.log
```

### 端口检查
```bash
# Linux
lsof -i :8080

# Windows
netstat -ano | findstr :8080
```

### 内存检查
```bash
# Linux
free -h

# Windows
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10
```

---

## 📊 环境变量

### 服务器配置
```bash
export MODE=local          # local, cloud, hybrid
export PORT=8080
export HOST=0.0.0.0
export RUST_LOG=info       # error, warn, info, debug, trace
```

### API 密钥
```bash
export OPENAI_API_KEY=sk-...
export ALIYUN_API_KEY=...
export ALIYUN_API_SECRET=...
```

### 路径配置
```bash
export SDKWORK_TTS_DATA=$HOME/.sdkwork-tts/data
export SDKWORK_TTS_CONFIG=$HOME/.sdkwork-tts/config
```

---

## 📁 目录结构

```
~/.sdkwork-tts/
├── bin/
│   ├── sdkwork-tts          # 主程序
│   ├── start_server.sh      # 启动脚本
│   ├── verify_install.sh    # 验证脚本
│   └── diagnose.sh          # 诊断工具
├── data/
│   ├── checkpoints/         # 模型文件
│   └── speaker_library/     # Speaker 库
└── config/
    └── server.yaml          # 配置文件
```

---

## 🛠️ 脚本工具

| 脚本 | 用途 |
|------|------|
| `install.sh` | 安装 |
| `uninstall.sh` | 卸载 |
| `upgrade.sh` | 升级 |
| `verify_install.sh` | 验证安装 |
| `generate_config.sh` | 生成配置 |
| `diagnose.sh` | 故障诊断 |
| `start_server.sh` | 启动服务器 |

---

## 🐛 常见问题

### 端口被占用
```bash
# 使用不同端口
sdkwork-tts server --port 8081
```

### 权限问题
```bash
# Linux
chmod +x ~/.sdkwork-tts/bin/sdkwork-tts

# Windows (管理员 PowerShell)
```

### 内存不足
```yaml
# server.yaml
local:
  batch_size: 1
  max_concurrent: 2
```

### Docker 无法启动
```bash
# 查看日志
docker logs sdkwork-tts

# 重新创建
docker compose down && docker compose up -d
```

---

## 📞 获取帮助

- **文档**: `docs/`
- **快速入门**: `GETTING_STARTED.md`
- **部署指南**: `docs/DEPLOYMENT_GUIDE.md`
- **GitHub Issues**: https://github.com/sdkwork-ai/sdkwork-tts/issues
- **Discussions**: https://github.com/sdkwork-ai/sdkwork-tts/discussions

---

**版本**: 1.0.0  
**更新日期**: 2026-02-21
