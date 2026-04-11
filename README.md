# 🤖 Hermes Agents for Dummies

> **Zero-to-Hero Documentation for Deploying Autonomous AI Swarms**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://python.org)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://docker.com)

## 📚 What This Is

This repository documents the complete journey of setting up a **Hermes multi-agent system** — from initial installation to running autonomous AI swarms. Think of it as the missing manual that prevents you from drowning in configuration hell.

The system features:
- 🤖 **Hermes Agent** — Central orchestrator
- 🐳 **Docker Sandboxes** — Isolated execution environments
- 🧠 **Model Router** — Intelligent AI model selection
- 🔄 **GitHub Integration** — Code management & CI/CD
- 📡 **Multi-Platform** — Telegram, Discord, Slack support

---

## 🗺️ Table of Contents

1. [Prerequisites](docs/01-prerequisites.md)
2. [Installation](docs/02-installation.md)
3. [Configuration](docs/03-configuration.md)
4. [First Agent Spawn](docs/04-first-agent.md)
5. [GitHub Integration](docs/05-github-integration.md)
6. [Model Routing](docs/06-model-routing.md)
7. [Troubleshooting](troubleshooting/README.md)
8. [Advanced Topics](docs/07-advanced.md)

---

## 🎯 Quick Start (If You're Impatient)

```bash
# 1. Clone this repo
git clone https://github.com/0x-wzw/hermes-agents-for-dummies.git
cd hermes-agents-for-dummies

# 2. Set up environment
cp .env.example .env
# Edit .env with your credentials

# 3. Run setup script
chmod +x scripts/setup.sh
./scripts/setup.sh

# 4. Start Hermes
hermes init
hermes start

# 5. Your first agent
curl -X POST http://localhost:9001/spawn \
  -H "Content-Type: application/json" \
  -d '{"task": "Hello World", "model": "ministral-3:8b"}'
```

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                   HERMES AGENT                          │
│              (Central Orchestrator)                     │
└──────────────┬────────────────────────────┬─────────────┘
               │                            │
     ┌─────────┴──────────┐       ┌───────┴────────┐
     │   Docker Sandbox   │       │ GitHub         │
     │   (Execution)    │       │ Integration    │
     └─────────┬──────────┘       └────────┬───────┘
               │                             │
     ┌─────────┴──────────┐       ┌───────┴────────┐
     │   Model Router     │       │  Skills        │
     │   (Ollama Cloud)   │       │  (50+)         │
     └────────────────────┘       └────────────────┘
```

---

## 🚨 Important: Security Notice

> ⚠️ **NEVER commit real credentials to this repository!**

This repo uses placeholders for sensitive data:
- API Keys: `[YOUR_API_KEY_HERE]`
- Tokens: `[YOUR_GITHUB_TOKEN_HERE]`
- Passwords: `[YOUR_PASSWORD_HERE]`

See [Environment Setup](docs/03-configuration.md#environment-variables) for secure credential management.

---

## 📦 System Requirements

### Minimum Specs
- **OS**: Linux (Ubuntu 22.04+), macOS, WSL2
- **RAM**: 8GB (16GB+ recommended for multi-agent)
- **Storage**: 20GB free space
- **Internet**: Stable connection for model downloads

### Optional (Recommended)
- **Docker**: For sandboxed execution
- **GitHub CLI**: For advanced GitHub integration
- **Ollama**: For local model hosting

---

## 🎓 Learning Path

| Stage | Time | Goal |
|-------|------|------|
| **Stage 1** | 30 min | Get Hermes running |
| **Stage 2** | 1 hour | Spawn your first agent |
| **Stage 3** | 2 hours | Connect GitHub & Telegram |
| **Stage 4** | 4 hours | Run multi-agent swarms |
| **Stage 5** | Ongoing | Build custom skills |

---

## 🛠️ What's Included

### Documentation (`docs/`)
- Step-by-step installation guides
- Configuration templates
- Troubleshooting runbooks
- Advanced topics (swarms, MCP, etc.)

### Scripts (`scripts/`)
- `setup.sh` — One-command installation
- `backup.sh` — Automated backups
- `health-check.sh` — System diagnostics
- `model-test.sh` — Test all available models

### Examples (`examples/`)
- Simple agent spawn
- Multi-agent workflow
- GitHub PR automation
- Custom skill creation

---

## 🧩 Key Integrations

| Platform | Status | Documentation |
|----------|--------|---------------|
| **GitHub** | ✅ Full | [GitHub Setup](docs/05-github-integration.md) |
| **Telegram** | ✅ Full | [Messaging Setup](docs/03-configuration.md#telegram) |
| **Discord** | ✅ Full | [Messaging Setup](docs/03-configuration.md#telegram) |
| **Ollama** | ✅ Full | [Model Routing](docs/06-model-routing.md) |
| **Docker** | ✅ Full | [Sandbox Guide](docs/07-advanced.md#docker-sandbox) |

---

## 📝 Contributing

This is a personal documentation project, but contributions welcome:
- 🐛 Bug reports in issues
- 📝 Documentation improvements
- 💡 Feature suggestions

---

## 📜 License

MIT License — See [LICENSE](LICENSE) file.

---

## 🎖️ Author

**Z Teoh (0x-wzw)**
- GitHub: [@0x-wzw](https://github.com/0x-wzw)
- Project: [Capital Sentience](https://github.com/0x-wzw/Capital-Sentience)

---

## 💬 Disclaimer

This is educational documentation. AI agents are powerful tools. Use responsibly. Test in sandboxes before production. Don't let the agents take over (yet).

---

**Ready?** Start with [Prerequisites →](docs/01-prerequisites.md)
