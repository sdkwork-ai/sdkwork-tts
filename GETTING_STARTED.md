# SDKWork-TTS 快速入门指南

**5 分钟快速开始使用 TTS 服务器**

---

## 🚀 方式一：Docker 启动 (最快)

### 1. 启动服务器

```bash
# CPU 版本
docker compose --profile cpu up -d

# 或使用单个 Docker 命令
docker run -d -p 8080:8080 \
  -v $(pwd)/checkpoints:/app/checkpoints:ro \
  -v $(pwd)/speaker_library:/app/speaker_library \
  --name sdkwork-tts \
  ghcr.io/sdkwork-cloud/sdkwork-tts:latest-cpu
```

### 2. 验证运行

```bash
curl http://localhost:8080/health
```

**预期响应**:
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "mode": "local",
  "uptime": 10
}
```

### 3. 测试合成

```bash
curl -X POST http://localhost:8080/api/v1/synthesis \
  -H "Content-Type: application/json" \
  -d '{
    "text": "你好，这是测试文本",
    "speaker": "vivian",
    "channel": "local"
  }'
```

---

## 🚀 方式二：本地安装 (推荐开发使用)

### Linux/macOS

```bash
# 1. 一键安装
curl -fsSL https://raw.githubusercontent.com/sdkwork-ai/sdkwork-tts/main/scripts/install.sh | bash

# 2. 加载环境变量
source ~/.bashrc

# 3. 验证安装
sdkwork-tts --version

# 4. 启动服务器
sdkwork-tts server --mode local
```

### Windows

```powershell
# 1. 下载安装脚本
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/sdkwork-ai/sdkwork-tts/main/scripts/install.ps1" -OutFile "install.ps1"

# 2. 运行安装
.\install.ps1

# 3. 重启 PowerShell 后验证
sdkwork-tts --version

# 4. 启动服务器
sdkwork-tts server --mode local
```

---

## 🚀 方式三：源码编译

```bash
# 1. 克隆项目
git clone https://github.com/sdkwork-ai/sdkwork-tts.git
cd sdkwork-tts

# 2. 编译
cargo build --release

# 3. 运行
./target/release/sdkwork-tts server --mode local
```

---

## 📝 使用示例

### 1. 语音合成

```bash
# 基础合成
curl -X POST http://localhost:8080/api/v1/synthesis \
  -H "Content-Type: application/json" \
  -d '{
    "text": "你好，世界",
    "speaker": "vivian",
    "output_format": "wav"
  }' \
  --output output.wav
```

### 2. 列出 Speaker

```bash
curl http://localhost:8080/api/v1/speakers
```

### 3. 语音设计

```bash
curl -X POST http://localhost:8080/api/v1/voice/design \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello from designed voice",
    "voice_design": {
      "description": "A warm female voice",
      "gender": "female"
    }
  }'
```

### 4. 语音克隆

```bash
curl -X POST http://localhost:8080/api/v1/voice/clone \
  -H "Content-Type: application/json" \
  -d '{
    "text": "这是克隆的声音",
    "voice_clone": {
      "reference_audio": "reference.wav",
      "mode": "full"
    }
  }'
```

---

## 🔧 使用 Makefile (可选)

```bash
# 查看所有可用命令
make help

# 构建
make build

# 运行服务器
make run

# 运行测试
make test

# Docker 启动
make docker-run

# 安装
make install
```

---

## ⚙️ 配置选项

### 环境变量

```bash
# 服务器配置
export MODE=local        # local, cloud, hybrid
export PORT=8080
export HOST=0.0.0.0

# Cloud API Keys
export OPENAI_API_KEY=sk-...
export ALIYUN_API_KEY=...
```

### 配置文件

创建 `server.yaml`:

```yaml
mode: local

local:
  enabled: true
  use_gpu: false
  batch_size: 4

speaker_lib:
  enabled: true
  local_path: speaker_library
```

启动时指定配置：

```bash
sdkwork-tts server --config server.yaml
```

---

## 🐛 故障排查

### 端口被占用

```bash
# 查看占用端口的进程
lsof -i :8080

# 使用不同端口
sdkwork-tts server --port 8081
```

### Docker 容器无法启动

```bash
# 查看日志
docker logs sdkwork-tts

# 重新创建容器
docker compose down
docker compose up -d
```

### 命令找不到

```bash
# Linux/macOS - 添加到 PATH
export PATH="$HOME/.sdkwork-tts/bin:$PATH"

# Windows - 添加到系统 PATH
[Environment]::SetEnvironmentVariable(
  "Path",
  $env:Path + ";$HOME\sdkwork-tts\bin",
  "User"
)
```

---

## 📚 下一步

- 📖 阅读完整文档：`docs/DEPLOYMENT_GUIDE.md`
- 💻 查看示例：`examples/`
- 🔧 配置指南：`server.example.yaml`
- 🐳 Docker 部署：`docker-compose.yml`

---

## 🆘 获取帮助

- **文档**: `/docs/`
- **问题**: [GitHub Issues](https://github.com/sdkwork-ai/sdkwork-tts/issues)
- **讨论**: [GitHub Discussions](https://github.com/sdkwork-ai/sdkwork-tts/discussions)

---

**恭喜！您已成功启动 TTS 服务器！** 🎉
