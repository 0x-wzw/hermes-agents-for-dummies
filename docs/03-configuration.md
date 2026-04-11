# 03 - Configuration

Complete configuration guide for Hermes Agent.

---

## 🔐 Security Warning

**NEVER commit credentials to git!**

This repository uses placeholders like:
- `[YOUR_GITHUB_TOKEN_HERE]`
- `[YOUR_TELEGRAM_BOT_TOKEN_HERE]`
- `[YOUR_API_KEY_HERE]`

Replace these with your actual values in your local `.env` file.

---

## 📝 Environment Variables

Create a `.env` file in your project root:

```bash
# Copy template
cp .env.example .env

# Edit
nano .env
```

### Required Variables

#### GitHub Integration
```bash
# GitHub Personal Access Token
# Get from: https://github.com/settings/tokens
GITHUB_TOKEN=[YOUR_GITHUB_TOKEN_HERE]

# Git Identity (match your GitHub account)
GIT_USER_NAME="Your Name"
GIT_USER_EMAIL="your.email@example.com"
```

#### Telegram (Optional)
```bash
# Create bot via @BotFather
telegram__api_key=[YOUR_TELEGRAM_BOT_TOKEN_HERE]
telegram__chat_id=[YOUR_CHAT_ID_HERE]
```

#### Ollama / Model Routing
```bash
# Default model (already active)
OLLAMA_API_URL=http://localhost:11434
DEFAULT_MODEL=kimi-k2.5:cloud

# Optional: API keys for cloud providers
OLLAMA_CLOUD_TOKEN=[YOUR_OLLAMA_TOKEN_HERE]
```

#### Docker Sandbox
```bash
# Enable isolated execution
ENABLE_DOCKER_SANDBOX=true
DOCKER_SOCKET=/var/run/docker.sock
```

### Optional Integrations

#### Tavily (Web Search)
```bash
# Get from: tavily.com
TAVILY_API_KEY=[YOUR_TAVILY_API_KEY_HERE]
```

#### Notion (Documentation)
```bash
# Notion Integration Token
NOTION_TOKEN=[YOUR_NOTION_TOKEN_HERE]
NOTION_DATABASE_ID=[YOUR_DATABASE_ID_HERE]
```

#### Linear (Project Management)
```bash
LINEAR_API_KEY=[YOUR_LINEAR_API_KEY_HERE]
```

#### HuggingFace
```bash
# For model downloads
HF_TOKEN=[YOUR_HUGGINGFACE_TOKEN_HERE]
```

---

## 📧 Git Configuration

### Method 1: Credential Store (Recommended)
```bash
# Store in ~/.git-credentials
git config --global credential.helper store

# Create credentials file
echo "https://[USERNAME]:[YOUR_GITHUB_TOKEN_HERE]@github.com" > ~/.git-credentials
chmod 600 ~/.git-credentials
```

### Method 2: SSH Key
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "[YOUR_EMAIL_HERE]" -f ~/.ssh/id_ed25519

# Add to GitHub: https://github.com/settings/keys
cat ~/.ssh/id_ed25519.pub

# Configure git
git config --global user.name "[YOUR_GIT_USERNAME]"
git config --global user.email "[YOUR_EMAIL_HERE]"
```

---

## 🤖 Model Configuration

### Available Models (38 total)

Create `~/.hermes/config/models.yaml`:

```yaml
models:
  # Fast/Lightweight
  ministral-3:3b:
    size: "5GB"
    strength: "quick_tasks"
    token_cost: 0.001
    
  ministral-3:8b:
    size: "10GB"
    strength: "general"
    token_cost: 0.002
    
  # Coding Specialist
  deepseek-v3.1:671b:
    size: "688GB"
    strength: "coding"
    token_cost: 0.05
    
  qwen3-coder:480b:
    size: "510GB"
    strength: "coding"
    token_cost: 0.04
    
  # Reasoning/Analysis
  kimi-k2-thinking:
    size: "1.1TB"
    strength: "reasoning"
    token_cost: 0.08
    
  cogito-2.1:671b:
    size: "689GB"
    strength: "philosophy"
    token_cost: 0.06
    
  # Default
  kimi-k2.5:
    size: "1.1TB"
    strength: "general"
    token_cost: 0.08
    default: true

# Routing rules
routing:
  code_review: deepseek-v3.1:671b
  debug: qwen3-coder:480b
  reasoning: kimi-k2-thinking
  quick_chat: ministral-3:8b
  default: kimi-k2.5
```

---

## 📱 Telegram Setup

### Step 1: Create Bot

1. Message [@BotFather](https://t.me/botfather)
2. Send `/newbot`
3. Follow prompts, save the token

### Step 2: Get Chat ID

```python
# Quick way: Send any message to your bot
# Then visit:
# https://api.telegram.org/bot[YOUR_BOT_TOKEN]/getUpdates

# Look for: "chat":{"id":123456789
```

### Step 3: Configure

```bash
# Add to .env
telegram__api_key=[YOUR_BOT_TOKEN_HERE]
telegram__chat_id=[YOUR_CHAT_ID_HERE]

# Test
curl -X POST "https://api.telegram.org/bot[YOUR_BOT_TOKEN]/sendMessage" \
  -d "chat_id=[YOUR_CHAT_ID]" \
  -d "text=Hermes configured!"
```

---

## 💬 Messenger Integrations

### Discord
```bash
# Add to .env
discord__bot_token=[YOUR_DISCORD_BOT_TOKEN_HERE]
discord__channel_id=[YOUR_CHANNEL_ID_HERE]

# Get token from: https://discord.com/developers/applications
```

### Slack
```bash
# Add to .env
slack__bot_token=[YOUR_SLACK_BOT_TOKEN_HERE]
slack__channel_id=[YOUR_CHANNEL_ID_HERE]

# Create app: https://api.slack.com/apps
```

---

## 🐳 Docker Sandbox Config

Create `~/.hermes/config/sandbox.yaml`:

```yaml
sandbox:
  enabled: true
  image: "python:3.11-slim"
  memory_limit: "2g"
  cpu_limit: "1.0"
  timeout: 300
  network_mode: "none"  # isolated
  
  # Allowed volumes (read-only)
  volumes:
    - "/app/shared:/shared:ro"
    - "/app/data:/data:rw"
  
  # Security
  security_opt:
    - "no-new-privileges:true"
  read_only: true
  
  # Resource monitoring
  monitoring:
    enabled: true
    log_resources: true
    max_memory_percent: 80
```

---

## 🧠 Memory Configuration

Create `~/.hermes/config/memory.yaml`:

```yaml
memory:
  # User memory
  user_store: "~/.hermes/memory/user.json"
  
  # Session memory (ephemeral)
  session_store: "~/.hermes/memory/session.json"
  
  # Long-term storage
  longterm_store: "~/.hermes/memory/longterm.json"
  
  # Backup
  backup:
    enabled: true
    interval: "daily"
    location: "~/.hermes/backups/memory/"
    
  # Sync (if using multiple devices)
  sync:
    enabled: false
    remote: "github"  # Options: github, notion, git
```

---

## 🔄 Skill Management

Create `~/.hermes/config/skills.yaml`:

```yaml
skills:
  # Auto-load skills
  auto_load: true
  skills_dir: "~/.hermes/skills/"
  
  # Remote skill registry
  registry:
    enabled: true
    url: "https://skills.hermes.dev/v1"
    
  # Custom skills
  custom:
    - name: "my-custom-skill"
      path: "~/.hermes/skills/custom/"
      auto_update: false
      
  # Skill permissions
  permissions:
    allow_file_write: true
    allow_network: true
    allow_shell: false  # Require confirmation
```

---

## 📝 Example .env File

```bash
# =====================================
# Hermes Agent Configuration
# =====================================

# Required: GitHub
GITHUB_TOKEN=[YOUR_GITHUB_TOKEN_HERE]
GIT_USER_NAME="Your Name"
GIT_USER_EMAIL="your.email@example.com"

# Optional: Telegram
telegram__api_key=[YOUR_TELEGRAM_BOT_TOKEN_HERE]
telegram__chat_id=[YOUR_CHAT_ID_HERE]

# Optional: Discord
discord__bot_token=[YOUR_DISCORD_BOT_TOKEN_HERE]
discord__channel_id=[YOUR_CHANNEL_ID_HERE]

# Optional: Slack
slack__bot_token=[YOUR_SLACK_BOT_TOKEN_HERE]
slack__channel_id=[YOUR_CHANNEL_ID_HERE]

# Model Routing
OLLAMA_API_URL=http://localhost:11434
DEFAULT_MODEL=kimi-k2.5:cloud

# Web Search
TAVILY_API_KEY=[YOUR_TAVILY_API_KEY_HERE]

# Docker
ENABLE_DOCKER_SANDBOX=true
DOCKER_SOCKET=/var/run/docker.sock

# Logging
LOG_LEVEL=info
LOG_FILE=~/.hermes/logs/hermes.log
```

**Save this as** `.env` (not `.env.example` which is in git)

---

## ✅ Configuration Checklist

- [ ] `.env` file created with your credentials
- [ ] GitHub token has `repo` scope
- [ ] Telegram bot created and tested
- [ ] Model routing configured
- [ ] Docker sandbox enabled (optional)
- [ ] Memory backup configured
- [ ] Skills directory set up
- [ ] `.env` added to `.gitignore` ⚠️

---

## 🧪 Test Configuration

```bash
# Test GitHub connectivity
curl -H "Authorization: token [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user | grep login

# Test Telegram
curl -X POST "https://api.telegram.org/bot[YOUR_BOT_TOKEN]/getMe"

# Test Ollama
curl http://localhost:11434/api/tags
```

---

## 🎯 Next Steps

[→ First Agent Spawn](04-first-agent.md)
