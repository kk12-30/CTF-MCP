# CTF-MCP: AI Agent Interface for Kali Linux

<div align="center">

```
  ____ _____ _____     __  __  ____ ____    ____  _____ ______     _______ ____  
 / ___|_   _|  ___|   |  \/  |/ ___|  _ \  / ___|| ____|  _ \ \   / / ____|  _ \ 
| |     | | | |_ _____| |\/| | |   | |_) | \___ \|  _| | |_) \ \ / /|  _| | |_) |
| |___  | | |  _|_____| |  | | |___|  __/   ___) | |___|  _ < \ V / | |___|  _ < 
 \____| |_| |_|       |_|  |_|\____|_|     |____/|_____|_| \_\ \_/  |_____|_| \_\
```

[![Go Report Card](https://goreportcard.com/badge/github.com/kk12-30/CTF-MCP)](https://goreportcard.com/report/github.com/kk12-30/CTF-MCP)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Kali%20Linux%20%7C%20Windows%20%7C%20macOS-lightgrey.svg)](https://www.kali.org/)

**CTF-MCP** 是一个专为 **Capture The Flag (CTF)** 竞赛设计的 Model Context Protocol (MCP) 实现。它充当 AI Agent (如 Claude, Cursor) 与 Kali Linux 渗透测试环境之间的桥梁，使 AI 能够直接调用专业的安全工具进行自动化解题。

[特性](#特性) • [架构](#架构) • [安装](#安装) • [使用](#使用) • [配置](#配置) • [免责声明](#免责声明)

</div>

## ✨ 特性

- **🔗 MCP 协议深度集成**: 完美支持 Model Context Protocol，让 AI Agent 理解并使用 Kali 工具链。
- **📊 实时进度可视化**: 服务端内置彩色 ASCII 进度条，实时展示命令执行进度、ETA、速度和输出量。
- **🔄 智能交互模式**: 支持 `interactive` 模式，通过 PTY 模拟处理如 `msfconsole`, `gdb`, `sqlmap` 等交互式工具。
- **📡 远程架构设计**: 采用 C/S 架构，服务端运行在 Kali 虚拟机/VPS，客户端运行在本地 IDE 环境，无缝连接。
- **📈 进程仪表盘**: 提供 HTTP API (`/api/dashboard`) 实时监控所有后台任务状态。
- **📝 CTF 知识库**: 内置 Web, Reverse, Pwn, Crypto, Misc 等分类的解题技能树文档。

## 🏗 架构

项目由两部分组成：

1.  **CTF Server (`CTF_server`)**: 
    - 运行在 Kali Linux 上。
    - 负责实际执行命令。
    - 管理进程生命周期和进度追踪。
    - 提供 HTTP API 供客户端调用。

2.  **CTF Client (`CTF_client`)**:
    - 运行在用户本地 (IDE/Desktop 环境)。
    - 实现 MCP 协议标准。
    - 将 AI 的工具调用请求转发给 CTF Server。
    - 将执行结果和日志回传给 AI。

## 💻 使用

### 服务端 (Kali Linux)

```bash
# 启动服务 (默认端口 6666)
./ctf_mcp_server

# 高级启动参数
./ctf_mcp_server -port 8080 -timeout 300 -debug
```


### 客户端 (MCP Configuration)

将 `mcp_client` 配置到你的 AI 工具中 (如 Claude Desktop 或 Cursor)。

**Claude Desktop 配置 (`claude_desktop_config.json`):**
```json
{
  "mcpServers": {
    "CTF-agent": {
      "command": "D:/ctf_mcp_win_amd64.exe",
      "args": ["-server", "http://<KALI_IP>:6666"]
    }
  }
}
```

**命令行测试:**
```bash
ctf_mcp_win_amd64.exe -server http://192.168.1.100:6666
```

## ⚠️ 免责声明

本工具仅供网络安全教育、CTF 竞赛和授权渗透测试使用。请勿将本工具用于未经授权的攻击行为。开发者不对因使用本工具造成的任何直接或间接后果承担责任。

