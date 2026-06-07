<!-- markdownlint-disable MD041 -->
# 🤖 Hermes Agents for Dummies — v2

> **Zero-to-Hero Documentation for Deploying Hermes Agent v0.16.0 — Autonomous AI Agents Powered by Nous Research**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![Hermes](https://img.shields.io/badge/Hermes-v0.16.0-purple.svg)](https://github.com/NousResearch/hermes-agent)
[![v2](https://img.shields.io/badge/Version-2.0.0-brightgreen.svg)](https://github.com/0x-wzw/hermes-agents-for-dummies)

## 📚 What This Is

This repository documents the complete journey of setting up a **Hermes Agent v0.16.0** (by Nous Research) — from installation to running autonomous AI agents. Think of it as the missing manual that prevents you from drowning in configuration hell.

The system features:
- 🤖 **Hermes Agent v0.16.0** — Central orchestrator from Nous Research
- 🧠 **Model Router** — Intelligent AI model selection via Ollama Cloud (41+ models)
- 🔄 **GitHub Integration** — Autonomous code management & CI/CD
- 📡 **Telegram & Notion Integration** — Messaging and documentation
- 🛡️ **Tirith Security** — Built-in safety layer
- 🧩 **50+ Built-in Skills** — Extensible plugin system

> **⚠️ Note:** This repo documents the setup process — actual configurations and secrets live in the private backup repository.

---

## 🗺️ Table of Contents

| # | Document | Description |
|---|----------|-------------|
| 1 | [Prerequisites](docs/01-prerequisites.md) | System requirements & accounts |
| 2 | [Installation](docs/02-installation.md) | Install Hermes Agent |
| 3 | [Configuration](docs/03-configuration.md) | Environment & config setup |
| 4 | [First Agent](docs/04-first-agent.md) | Your first autonomous agent |
| 5 | [GitHub Integration](docs/05-github-integration.md) | GitHub + Hermes |
| 6 | [Model Routing](docs/06-model-routing.md) | Ollama Cloud model selection |
| 7 | [Troubleshooting](troubleshooting/README.md) | Common issues & fixes |
| 8 | [Advanced Topics](docs/07-advanced.md) | Cron, kanban, SOUL identity |

---

## 🎯 Quick Start (If You're Impatient)

```bash
# 1. Install Hermes Agent
pip install hermes-agent

# 2. Set up environment
cp .env.example .env
# Edit .env with your credentials (placeholders only!)

# 3. Run the setup wizard
hermes setup

# 4. Your first agent
hermes run "say hello world in one sentence"
```

That's it. Three commands. No Docker required.

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                   HERMES AGENT v0.16.0                  │
│              (Central Orchestrator)                     │
└──────────────┬────────────────────────────┬─────────────┘
               │                            │
     ┌─────────┴──────────┐       ┌───────┴────────┐
     │   Ollama Cloud     │       │ GitHub         │
     │   (41+ Models)     │       │ Integration    │
     └─────────┬──────────┘       └────────┬───────┘
               │                            │
     ┌─────────┴──────────┐       ┌───────┴────────┐
     │   Model Router     │       │  Skills        │
     │   (Auto-Routing)   │       │  (50+)         │
     └────────────────────┘       └────────────────┘

     ┌────────────────────┐       ┌────────────────┐
     │   Tirith Security  │       │  Telemetry     │
     │   (Safety Layer)   │       │  (Logging)     │
     └────────────────────┘       └────────────────┘
```

---

## 🚨 Important: Security Notice

> ⚠️ **NEVER commit real credentials to this repository!**
> This is a **PUBLIC** repository.

This repo uses placeholders for ALL sensitive data:
- API Keys: `[YOUR_VALUE_HERE]`
- Tokens: `[YOUR_GITHUB_TOKEN_HERE]`
- Passwords: `[YOUR_PASSWORD_HERE]`

See [Configuration](docs/03-configuration.md) for secure credential management.

---

## 📦 System Requirements

### Minimum Specs
| Component | Requirement |
|-----------|-------------|
| **OS** | Linux (Ubuntu 22.04+), macOS 12+, WSL2 |
| **Python** | 3.11+ (required by Hermes Agent) |
| **RAM** | 4GB (8GB+ recommended) |
| **Storage** | 5GB for Hermes core |
| **Internet** | Stable connection for Ollama Cloud API |

### Optional
| Tool | Purpose |
|------|---------|
| **Git** | Version control & GitHub integration |
| **uv** | Fast Python package manager (preferred over pip) |
| **Docker** | Not required for Hermes Agent v0.16.0 |

---

## 🎓 Learning Path

| Stage | Time | Goal |
|-------|------|------|
| **Stage 1** | 15 min | Install Hermes & run setup wizard |
| **Stage 2** | 30 min | Spawn your first agent |
| **Stage 3** | 1 hour | Connect GitHub & Telegram |
| **Stage 4** | 2 hours | Configure model routing & cron jobs |
| **Stage 5** | Ongoing | Build custom skills & advanced workflows |

---

## 🛠️ What's Included

### Documentation (`docs/`)
- Step-by-step installation guides
- Configuration templates with credential-safe placeholders
- Troubleshooting runbooks
- Advanced topics (cron, kanban, SOUL identity, persistence)

### Configuration Templates
- `.env.example` — Environment variable template (all placeholders)
- `.gitignore` — Thorough secret protection
- `package.json` — CI tooling metadata (v2.0.0)

---

## 🧩 Key Integrations

| Platform | Status | Documentation |
|----------|--------|---------------|
| **GitHub** | ✅ Full | [GitHub Setup](docs/05-github-integration.md) |
| **Telegram** | ✅ Full | [Configuration](docs/03-configuration.md) |
| **Notion** | ✅ Full | [Configuration](docs/03-configuration.md) |
| **Ollama Cloud** | ✅ Full (41 models) | [Model Routing](docs/06-model-routing.md) |
| **Ollama Local** | ✅ Optional | [Model Routing](docs/06-model-routing.md) |
| **Tirith** | ✅ Security | [Advanced Topics](docs/07-advanced.md) |

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
- Other repos: [hermes-agents-for-dummies (private backup)](https://github.com/0x-wzw/hermes-agents-for-dummies-backup)

---

## 💬 Disclaimer

This is educational documentation. AI agents are powerful tools. Use responsibly. Test in sandboxes before production.

---

**Ready?** Start with [Prerequisites →](docs/01-prerequisites.md)