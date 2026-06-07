# 🚨 Troubleshooting Guide

Common issues and solutions for Hermes Agent v0.16.0.

---

## 🔥 Critical Issues

### Issue: "Hermes not found / command not found"

**Symptom:** `hermes --version` fails.

**Solution:**
```bash
# Check if installed
pip list | grep hermes-agent

# Reinstall
pip install hermes-agent

# Check Python version (must be 3.11+)
python3 --version

# Check if venv is active
which hermes
```

---

### Issue: "Permission denied (publickey)" — Git

**Symptom:** Cannot clone/pull from GitHub via SSH.

**Solution:**
```bash
# Option 1: Use HTTPS instead of SSH
git remote set-url origin https://github.com/[YOUR_USERNAME]/[REPO].git

# Option 2: Fix SSH keys
ssh-add ~/.ssh/id_ed25519

# Option 3: Generate new SSH key
ssh-keygen -t ed25519 -C "[YOUR_EMAIL_HERE]"
cat ~/.ssh/id_ed25519.pub
# Add to GitHub: https://github.com/settings/keys
```

---

### Issue: "Git authentication failed"

**Symptom:** Git push/pull fails with 401.

**Solution:**
```bash
# Check GITHUB_TOKEN is set in .env
grep GITHUB_TOKEN .env

# Test token
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user

# If token expired, regenerate at:
# https://github.com/settings/tokens

# For git operations, use credential helper:
git config --global credential.helper store
```

---

## 🤖 Model Issues

### Issue: "Ollama Cloud connection failed"

**Symptom:** Cannot connect to Ollama Cloud to run models.

**Solution:**
```bash
# Check API key is set
grep OLLAMA_API_KEY .env

# Test connection
curl -H "Authorization: Bearer [YOUR_OLLAMA_API_KEY]" \
  https://api.ollama.com/api/tags

# If fails, regenerate key at:
# https://ollama.com/settings/keys

# Check network connectivity
curl -v https://api.ollama.com
```

---

### Issue: "Model not found" / "Invalid model"

**Symptom:** Specified model name not recognized.

**Solution:**
```bash
# List available models
curl -H "Authorization: Bearer [YOUR_OLLAMA_API_KEY]" \
  https://api.ollama.com/api/tags

# Use correct model name (e.g., deepseek-v4-flash)
hermes run "hello" --model "deepseek-v4-flash"

# Check for typos in model name
```

---

### Issue: "Model timeout"

**Symptom:** Model taking too long to respond.

**Solution:**
```bash
# Increase timeout
hermes run "complex analysis" --timeout 600

# Use faster model
hermes run "complex analysis" --model "mistral-small-24b" --timeout 300

# Check network latency
ping api.ollama.com
```

---

## 🔐 Authentication Issues

### Issue: "401 Bad Credentials" — GitHub

**Solution:**
```bash
# Regenerate GitHub token
# https://github.com/settings/tokens

# Update .env
GITHUB_TOKEN=[YOUR_NEW_GITHUB_TOKEN_HERE]

# Test with Bearer auth
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user
```

---

### Issue: "Telegram bot not responding"

**Solution:**
```bash
# Test bot
curl -X POST "https://api.telegram.org/bot[YOUR_BOT_TOKEN]/getMe"

# Check bot token in .env
grep TELEGRAM_BOT_TOKEN .env

# Send test message
curl -X POST "https://api.telegram.org/bot[YOUR_BOT_TOKEN]/sendMessage" \
  -d "chat_id=[YOUR_CHAT_ID]" \
  -d "text=Test from troubleshooting"
```

---

### Issue: "Notion API not working"

**Solution:**
```bash
# Check API key
grep NOTION_API_KEY .env

# Test connection
curl -H "Authorization: Bearer [YOUR_NOTION_API_KEY]" \
  -H "Notion-Version: 2022-06-28" \
  https://api.notion.com/v1/users/me

# Ensure integration is shared with your workspace
# https://www.notion.so/my-integrations
```

---

## 🏠 Profile Issues

### Issue: "Profile not found"

**Symptom:** `hermes profile switch "work"` fails.

**Solution:**
```bash
# List available profiles
hermes profile list

# Create profile
hermes profile create "work"

# Switch
hermes profile switch "work"

# Check active profile
hermes profile current
```

---

### Issue: "Vault not initialized"

**Symptom:** `hermes vault set` fails.

**Solution:**
```bash
# Initialize vault
hermes vault init

# Set a secret
hermes vault set "my-secret" "my-value"

# Verify
hermes vault list
```

---

## 📡 Network Issues

### Issue: "Connection refused"

**Solution:**
```bash
# Check if service is running
hermes --version

# Check DNS
nslookup api.ollama.com

# Check firewall
sudo ufw status

# Test network
curl -v https://api.ollama.com
```

---

### Issue: "DNS resolution failed"

**Solution:**
```bash
# Test DNS
nslookup github.com
nslookup api.ollama.com

# Check /etc/resolv.conf
cat /etc/resolv.conf

# Try different DNS
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

---

## 📝 Logging & Debugging

### Enable Debug Mode

```bash
# Set in .env
LOG_LEVEL=debug

# Or runtime
export LOG_LEVEL=debug
hermes run "test" --verbose
```

### View Logs

```bash
# View recent logs
tail -f ~/.hermes/logs/hermes.log

# Check setup status
hermes setup --status
```

### Collect Diagnostics

```bash
#!/bin/bash
# diagnostics.sh

echo "=== Hermes Diagnostics ===" > diagnostics.txt

echo "
--- Version ---" >> diagnostics.txt
hermes --version >> diagnostics.txt 2>&1

echo "
--- Setup Status ---" >> diagnostics.txt
hermes setup --status >> diagnostics.txt 2>&1

echo "
--- Environment ---" >> diagnostics.txt
env | grep -E "GITHUB|OLLAMA|TELEGRAM|NOTION|LOG_" >> diagnostics.txt 2>&1

echo "
--- Recent Logs ---" >> diagnostics.txt
tail -50 ~/.hermes/logs/hermes.log 2>/dev/null >> diagnostics.txt

echo "
--- Disk Usage ---" >> diagnostics.txt
df -h . >> diagnostics.txt

echo "
--- Memory ---" >> diagnostics.txt
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
# Reinstall from scratch
pip uninstall hermes-agent -y
pip install hermes-agent

# Reset Hermes config
rm -rf ~/.hermes/config.yaml

# Re-run setup
hermes setup

# Reconfigure .env
cp .env.example .env
nano .env
```

---

## 📚 Quick Reference Card

| Issue | Quick Fix |
|-------|-----------|
| Hermes not found | `pip install hermes-agent` |
| Model connection failed | Check `OLLAMA_API_KEY` in `.env` |
| Git auth failed | Regenerate token, update `.env` |
| Telegram not responding | Check `TELEGRAM_BOT_TOKEN` |
| Profile missing | `hermes profile create "name"` |
| Vault not initialized | `hermes vault init` |
| Network error | Check firewall/DNS/API key |

---

**Last resort:** `pip uninstall hermes-agent -y && pip install hermes-agent && hermes setup` 💀