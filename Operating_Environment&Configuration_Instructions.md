# 运行环境与配置说明

## 后端开发指南
### 1. 基础设施 
本项目采用 **Docker** 实现容器化部署，以确保开发与生产环境的一致性。
* **数据库及中间件**：项目依赖 **MySQL** 与 **Redis**。
* **快速启动**：请参考根目录下的 `docker-compose.yml` 配置文件。通过执行 `docker-compose up -d` 即可快速拉取并启动所需的基础服务镜像。
### 2. AI 服务集成 
本项目深度集成 **阿里云 (Alibaba Cloud)** 的大模型能力，涵盖了多模态交互的核心功能：
* **LLM (大语言模型)**：核心对话逻辑驱动。
* **TTS & ASR**：实现语音合成与实时语音识别。
* **RAG (知识库)**：基于检索增强生成的定制化知识检索。
### 3. 配置指南 
在启动应用前，请完成以下配置步骤：
1. **环境变量**：参考 `.env.example` 文件，新建 `.env` 文件并填入您的 API 密钥及环境参数。
2. **应用配置**：检查 `src/main/resources/application.properties`，确保数据库连接字符串与您的 Docker 宿主机配置匹配。
> [!IMPORTANT]
> **注意**：本项目目前针对阿里云服务接口进行了深度优化。若需切换至其他服务商（如 OpenAI, Azure 等），需对底层 Adapter 层代码进行适配性重构，建议优先使用默认配置进行 Demo 跑通。

<br>

## 前端开发指南
### 1. 运行环境
* **Node.js**: 建议版本 `v16.0.0` 或更高。
* **Package Manager**: 使用 Node.js 自带的 `npm` 进行依赖管理。
* **Backend Middleware**: 本地需运行 **WebSocket** 服务（默认：`ws://localhost:8080`），用于处理实时文本及音频流传输。

### 2. 快速启动 
#### Step 1: 依赖安装
```bash
npm install
```
#### Step 2: 环境配置
项目使用 Vite 进行构建，请配置对应的环境变量文件：
```bash
# 拷贝模板生成本地配置文件
copy .env.example .env.development
```
> **提示**：若后端地址有变动，请修改 `.env.development` 中的 `VITE_WS_URL` 字段。
#### Step 3: 启动开发服务器
```bash
npm run dev
```
启动后访问：`http://localhost:3000` (Vite 默认端口)。
---
###  WebSocket 交互规范 
为确保前后端正常通信，后端服务需满足以下约束：
* **连接握手**：路径格式应为 `/chat?characterId={id}&personaPrompt={prompt}`。
* **数据格式**：支持二进制流传输（用于音频 `ArrayBuffer` 交互）。
* **稳定性**：需实现标准的心跳机制 (`ping`/`pong`) 以维持长连接。
---
### 常用命令 (CLI Commands)
| 命令 | 说明 |
| --- | --- |
| `npm run build` | 构建生产环境产物 |
| `npm run preview` | 本地预览生产环境构建结果 |
| `npm run lint` | 执行代码规范检查 |
| `npm run format` | 自动化代码格式化 |
---

### 常见问题 
1. **WebSocket 异常**：请确认后端 8080 端口已放行，并检查浏览器控制台 (Network/Console) 的具体状态码。
2. **音频播放受限**：受浏览器安全策略 (Autoplay Policy) 限制，音频播放须由用户交互（如点击麦克风/发送）触发，请勿在无交互下自动调用播放接口。
3. **依赖冲突**：若安装报错，可尝试执行 `npm cache clean --force` 并重置 `node_modules` 文件夹。
