# 🚨 Troubleshooting Guide

Common issues and solutions for Hermes Agent.

---

## 🔥 Critical Issues

### Issue: "Port 9001 already in use"

**Problem:** Another service is using the port.

**Solution:**
```bash
# Find process
sudo lsof -i :9001
# or
sudo netstat -tulpn | grep 9001

# Kill process
sudo kill -9 <PID>

# Or use different port
docker run -p 9002:9001 hermes/ai-agent:latest
```

---

### Issue: "Docker daemon not running"

**Symptom:** Cannot connect to Docker

**Solution:**
```bash
# Linux
sudo systemctl start docker
sudo systemctl enable docker

# Check status
sudo systemctl status docker

# Add user to docker group
sudo usermod -aG docker $USER
# Logout and login again
```

---

### Issue: "Permission denied (publickey)" - Git

**Symptom:** Cannot clone/pull from GitHub

**Solution:**
```bash
# Option 1: Use HTTPS instead of SSH
git remote set-url origin https://github.com/[USER]/[REPO].git

# Option 2: Fix SSH keys
ssh-add ~/.ssh/id_ed25519

# Option 3: Generate new SSH key
ssh-keygen -t ed25519 -C "email@example.com"
cat ~/.ssh/id_ed25519.pub
# Add to GitHub: https://github.com/settings/keys
```

---

## 🤖 Model Issues

### Issue: "Model not found"

**Problem:** Requested model not available.

**Solution:**
```bash
# Check available models
curl http://localhost:11434/api/tags

# Pull missing model
curl -X POST http://localhost:11434/api/pull \
  -d '{"name": "ministral-3:8b"}'

# Check Ollama is running
docker ps | grep ollama
# or
ollama serve
```

---

### Issue: "Model timeout"

**Problem:** Model taking too long.

**Solution:**
```bash
# Increase timeout in request
curl -X POST http://localhost:9001/agent/spawn \
  -d '{"task": "...", "timeout": 600}'

# Use faster model
curl -X POST http://localhost:9001/agent/spawn \
  -d '{"task": "...", "model": "ministral-3:8b", "timeout": 60}'

# Check system resources
free -h
htop
df -h
```

---

## 🔐 Authentication Issues

### Issue: "401 Bad Credentials"

**Problem:** Token expired or invalid.

**Solution:**
```bash
# Regenerate GitHub token
# https://github.com/settings/tokens

# Update .env
export GITHUB_TOKEN=[NEW_TOKEN_HERE]

# Or update git credentials
echo "https://[USER]:[TOKEN]@github.com" > ~/.git-credentials
```

---

### Issue: "Telegram bot not responding"

**Solution:**
```bash
# Test bot
curl -X POST "https://api.telegram.org/bot[TOKEN]/getMe"

# Get chat ID
curl -X POST "https://api.telegram.org/bot[TOKEN]/getUpdates"

# Send test message
curl -X POST "https://api.telegram.org/bot[TOKEN]/sendMessage" \
  -d "chat_id=[CHAT_ID]" \
  -d "text=Test message"
```

---

## 🐳 Docker Issues

### Issue: "Container exited immediately"

**Solution:**
```bash
# Check logs
docker logs hermes-agent

# Run interactively for debugging
docker run -it hermes/ai-agent:latest /bin/bash

# Check environment variables
docker inspect hermes-agent | grep -A 10 Env
```

---

### Issue: "No space left on device"

**Solution:**
```bash
# Clean up Docker
docker system prune -a
docker volume prune

# Clean up old images
docker images -q | xargs docker rmi

# Check disk space
df -h
```

---

## 📡 Network Issues

### Issue: "Connection refused"

**Solution:**
```bash
# Check if service is running
sudo systemctl status hermes

# Check port
sudo lsof -i :9001

# Check firewall
sudo ufw status
sudo ufw allow 9001/tcp

# Test locally
curl http://localhost:9001/health
```

---

### Issue: "DNS resolution failed"

**Solution:**
```bash
# Test DNS
nslookup api.github.com
nslookup api.ollama.com

# Check /etc/resolv.conf
cat /etc/resolv.conf

# Restart network
sudo systemctl restart systemd-resolved
```

---

## 📝 Logging & Debugging

### Enable Debug Mode

```bash
# Set debug level
export HERMES_LOG_LEVEL=debug
export HERMES_LOG_FILE=/var/log/hermes/debug.log

# Run with verbose output
hermes start --verbose
```

### Get System Information

```bash
# Health check
curl http://localhost:9001/health

# Version info
curl http://localhost:9001/version

# Running agents
curl http://localhost:9001/agents

# System metrics
curl http://localhost:9001/metrics
```

### Collect Diagnostics

```bash
#!/bin/bash
# diagnostics.sh

echo "=== Hermes Diagnostics ===" > diagnostics.txt

echo "
--- Process List ---" >> diagnostics.txt
ps aux | grep hermes >> diagnostics.txt

echo "
--- Docker Containers ---" >> diagnostics.txt
docker ps >> diagnostics.txt

echo "
--- Recent Logs ---" >> diagnostics.txt
docker logs --tail 100 hermes-agent >> diagnostics.txt

echo "
--- Environment ---" >> diagnostics.txt
env | grep HERMES >> diagnostics.txt

echo "
--- Disk Usage ---" >> diagnostics.txt
df -h >> diagnostics.txt

echo "
--- Memory Usage ---" >> diagnostics.txt
free -h >> diagnostics.txt

echo "Diagnostics saved to diagnostics.txt"
```

---

## 🆘 Still Stuck?

### Get Help

1. **Check documentation**: [../README.md](../README.md)
2. **Search issues**: [GitHub Issues](https://github.com/0x-wzw/hermes-agents-for-dummies/issues)
3. **Create issue**: Include diagnostics.txt and logs

### Emergency Reset

```bash
# Stop everything
docker stop $(docker ps -q)
docker rm $(docker ps -aq)

# Reset config
rm -rf ~/.hermes/config/*
rm ~/.hermes/.env

# Reconfigure
cp ~/.hermes/.env.example ~/.hermes/.env
nano ~/.hermes/.env

# Restart
hermes init
hermes start
```

---

## 📚 Quick Reference Card

| Issue | Quick Fix |
|-------|-----------|
| Port conflict | `sudo lsof -i :9001 && sudo kill <PID>` |
| Model not found | `curl -X POST localhost:11434/api/pull -d '{"name":"model"}'` |
| Auth failed | Regenerate token, update .env |
| Out of space | `docker system prune -a` |
| Service down | `docker-compose restart` |
| Network error | Check firewall: `sudo ufw allow 9001/tcp` |

---

**Last resort:** Delete and reinstall 💀
