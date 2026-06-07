# 01 — Prerequisites

Before installing Hermes Agent v0.16.0, ensure your environment is ready.

---

## 🖥️ Operating System

| OS | Status | Notes |
|----|--------|-------|
| **Linux** (Ubuntu 22.04+ / Debian) | ✅ Recommended | Fully tested |
| **macOS** (12 Monterey+) | ✅ Supported | Homebrew recommended |
| **Windows** (WSL2) | ✅ Works | Ubuntu 22.04 LTS in WSL2 |
| Windows (native) | ❌ Not supported | Use WSL2 |

---

## 📦 Required Software

### 1. Python 3.11+

Hermes Agent v0.16.0 requires **Python 3.11 or higher**.

```bash
# Check version
python3 --version  # Must be 3.11+

# Ubuntu/Debian installation
sudo apt update
sudo apt install python3 python3-pip python3-venv

# macOS with Homebrew
brew install python@3.11

# Verify
python3 --version  # Should show 3.11.x or 3.12.x
```

### 2. Git (Required)

```bash
# Check installation
git --version

# Ubuntu/Debian
sudo apt install git

# macOS
brew install git

# Configure (use your name/email)
git config --global user.name "[YOUR_GIT_USERNAME]"
git config --global user.email "[YOUR_EMAIL_HERE]"
```

### 3. uv (Recommended Package Manager)

[uv](https://github.com/astral-sh/uv) is a fast Python package manager — preferred over pip for installing Hermes.

```bash
# Install uv (Linux/macOS)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or via pip
pip install uv

# Verify
uv --version
```

### 4. pip (Alternative)

If you prefer not to use uv, ensure pip is up to date:

```bash
python3 -m pip install --upgrade pip
```

---

## 🔐 Accounts & API Keys

You'll need accounts on the following services:

### 1. GitHub (Required)
| Item | Details |
|------|---------|
| **Account** | Free account: [github.com](https://github.com) |
| **Purpose** | Code storage, backup, CI/CD integration |
| **Token** | Classic PAT with `repo` + `workflow` scopes |

### 2. Ollama Cloud (Required for Model Inference)
| Item | Details |
|------|---------|
| **Account** | Free account: [ollama.com](https://ollama.com) |
| **Purpose** | Access to 41+ AI models (deepseek-v4-flash, etc.) |
| **API Key** | Generate at: [ollama.com/settings/keys](https://ollama.com/settings/keys) |

### 3. Telegram (Optional but Recommended)
| Item | Details |
|------|---------|
| **Bot** | Create via [@BotFather](https://t.me/botfather) |
| **Purpose** | Agent notifications, remote control |

### 4. Notion (Optional)
| Item | Details |
|------|---------|
| **Integration** | Create at: [notion.so/my-integrations](https://www.notion.so/my-integrations) |
| **Purpose** | Documentation & knowledge management |

---

## 💾 Storage Requirements

| Component | Minimum |
|-----------|---------|
| Hermes Core | 5GB |
| Logs & cache | 1GB |
| **Total** | **6GB** |

> **Note:** Hermes Agent v0.16.0 uses Ollama Cloud for inference — no local model downloads needed.
> This dramatically reduces storage requirements compared to local model hosting.

---

## 🌐 Network Requirements

### Outbound Connections Required

| Service | Port | Purpose |
|---------|------|---------|
| GitHub | 443/tcp | Code fetch/push |
| Ollama Cloud API | 443/tcp | Model inference |
| Telegram API | 443/tcp | Messaging |
| Notion API | 443/tcp | Documentation |
| PyPI | 443/tcp | Package installation |

### Firewall Settings

```bash
# If behind corporate firewall, whitelist:
- github.com
- api.ollama.com (or ollama.com)
- api.telegram.org
- api.notion.com (or www.notion.so)
- pypi.org
- files.pythonhosted.org
```

---

## 🧪 Quick Environment Check

Save this as `check-env.sh` and run to verify your setup:

```bash
#!/bin/bash
echo "=== Hermes v0.16.0 Environment Check ==="

# Check Python
python3 --version 2>/dev/null || echo "❌ Python not found"

# Check Git
git --version 2>/dev/null || echo "❌ Git not found"

# Check uv (preferred)
uv --version 2>/dev/null || echo "ℹ️  uv not found (pip alternative works)"

# Check curl
curl --version | head -1 || echo "❌ curl not found"

# Check disk space
DF=$(df -h . | tail -1 | awk '{print $4}')
echo "✅ Available disk space: $DF"

# Check memory
FREE=$(free -h 2>/dev/null | grep Mem | awk '{print $7}')
if [ -n "$FREE" ]; then
    echo "✅ Available memory: $FREE"
fi

echo "=== Check Complete ==="
```

```bash
chmod +x check-env.sh
./check-env.sh
```

---

## ✅ Pre-Flight Checklist

- [ ] Python 3.11+ installed and working
- [ ] Git configured with username/email
- [ ] GitHub account + Personal Access Token (PAT)
- [ ] Ollama Cloud account + API key
- [ ] Telegram bot created (optional)
- [ ] Notion integration set up (optional)
- [ ] 6GB+ free disk space
- [ ] Firewall allows HTTPS (443) outbound

---

## 🚀 Next Steps

Once all checks pass:

[→ Continue to Installation](02-installation.md)