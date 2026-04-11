# 01 - Prerequisites

Before diving into Hermes installation, ensure your environment is ready.

---

## 🖥️ Operating System

### Recommended: Linux
- **Ubuntu 22.04 LTS+** (tested & recommended)
- Debian-based distros
- CentOS/RHEL (with adjustments)

### macOS
- macOS 12 (Monterey) or later
- Homebrew recommended for dependencies

### Windows
- **WSL2 required** — Native Windows support limited
- Ubuntu 22.04 LTS in WSL2

---

## 📦 Required Software

### 1. Python 3.10+

```bash
# Check version
python3 --version  # Should be 3.10+

# Ubuntu/Debian installation
sudo apt update
sudo apt install python3 python3-pip python3-venv

# macOS with Homebrew
brew install python
```

### 2. Git

```bash
# Check installation
git --version

# Ubuntu/Debian
sudo apt install git

# Configure (use your name/email, not placeholders)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 3. Docker (Recommended)

```bash
# Install Docker
# Ubuntu:
sudo apt install docker.io docker-compose
sudo systemctl enable --now docker

# macOS:
brew install --cask docker

# Verify
docker --version
docker-compose --version

# Add user to docker group (logout required)
sudo usermod -aG docker $USER
```

### 4. curl

```bash
# Usually pre-installed, verify:
curl --version

# Ubuntu:
sudo apt install curl

# macOS:
brew install curl
```

---

## 🔐 Accounts & API Keys

You'll need accounts on the following services:

### 1. GitHub (Required)
- Free account sufficient
- Purpose: Code storage, GitHub Actions, CLI integration
- **Setup**: [github.com](https://github.com)

### 2. Telegram (Optional but Recommended)
- Bot account for messaging
- Purpose: Agent notifications, remote control
- **Setup**: [BotFather](https://t.me/botfather)

### 3. Ollama API Access (Optional)
- For multi-model routing
- Purpose: Access to 38+ AI models
- **Default**: Uses built-in kimi-k2.5

### 4. Tavily API (Optional)
- Web search capabilities
- Purpose: Agent web research
- **Setup**: [tavily.com](https://tavily.com)

---

## 💾 Storage Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| Hermes Core | 2GB | 5GB |
| Models (cached) | 10GB | 50GB+ |
| Docker Images | 5GB | 20GB |
| Logs | 1GB | 5GB |
| **Total** | **18GB** | **80GB+** |

---

## 🌐 Network Requirements

### Outbound Connections Required

| Service | Port | Purpose |
|---------|------|---------|
| GitHub | 443/tcp | Code fetch/push |
| Ollama API | 443/tcp | Model inference |
| Docker Hub | 443/tcp | Image pulls |
| Telegram API | 443/tcp | Messaging |

### Firewall Settings

```bash
# If behind corporate firewall, whitelist:
- github.com
- api.ollama.com
- api.telegram.org
- registry-1.docker.io
```

---

## 🧪 Quick Environment Check

Run this script to verify your setup:

```bash
#!/bin/bash

echo "=== Hermes Environment Check ==="

# Check Python
python3 --version 2>/dev/null || echo "❌ Python not found"

# Check Git
git --version 2>/dev/null || echo "❌ Git not found"

# Check Docker
docker --version 2>/dev/null || echo "❌ Docker not found"
docker-compose --version 2>/dev/null || echo "❌ Docker Compose not found"

# Check curl
curl --version | head -1 || echo "❌ curl not found"

# Check disk space
DF=$(df -h . | tail -1 | awk '{print $4}')
echo "✅ Available disk space: $DF"

# Check memory
FREE=$(free -h 2>/dev/null | grep Mem | awk '{print $7}')
if [ -n "$FREE" ]; then
    echo "✅ Available memory: $FREE"
else
    echo "ℹ️  Memory check: vm_stat or free not found"
fi

echo "=== Check Complete ==="
```

Save as `check-env.sh` and run:
```bash
chmod +x check-env.sh
./check-env.sh
```

---

## ✅ Pre-Flight Checklist

- [ ] Python 3.10+ installed
- [ ] Git configured with username/email
- [ ] Docker installed & running
- [ ] GitHub account created
- [ ] Telegram bot created (optional)
- [ ] 20GB+ free disk space
- [ ] Firewall allows HTTPS (443) outbound

---

## 🚨 Common Issues

### Issue: "Docker daemon not running"
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### Issue: "Permission denied on Docker"
```bash
sudo usermod -aG docker $USER
# Logout and login again
```

### Issue: "Python not found"
```bash
# Some systems use 'python' not 'python3'
# Create alias:
alias python=python3
# Or use full path:
/usr/bin/python3 --version
```

---

## 🎯 Next Steps

Once all checks pass:

[→ Continue to Installation](02-installation.md)
