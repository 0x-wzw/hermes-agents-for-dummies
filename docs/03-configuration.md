# 03 — Configuration

Complete configuration guide for Hermes Agent v0.16.0.

---

## 🔐 Security Warning

**NEVER commit credentials to git!**

This repository uses placeholders like:
- `[YOUR_GITHUB_TOKEN_HERE]`
- `[YOUR_TELEGRAM_BOT_TOKEN_HERE]`
- `[YOUR_OLLAMA_API_KEY_HERE]`

Replace these with your actual values ONLY in your local `.env` file (which is in `.gitignore`).

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

#### Git Identity
```bash
GIT_USER_NAME=[YOUR_GIT_USERNAME]
GIT_USER_EMAIL=[YOUR_EMAIL_HERE]
```

#### GitHub Integration
```bash
# GitHub Personal Access Token
# Get from: https://github.com/settings/tokens
GITHUB_TOKEN=[YOUR_GITHUB_TOKEN_HERE]
```

### Required for Model Inference

#### Ollama Cloud (Primary Provider)
```bash
# Get API key from: https://ollama.com/settings/keys
OLLAMA_API_KEY=[YOUR_OLLAMA_API_KEY_HERE]
```

#### Ollama Local (Alternative)
```bash
# Uncomment to use local Ollama instead of cloud
# OLLAMA_BASE_URL=http://localhost:11434/v1
# WARNING: OLLAMA_BASE_URL overrides the ollama-cloud provider!
```

### Optional Integrations

#### Telegram
```bash
# Create bot via @BotFather on Telegram
TELEGRAM_BOT_TOKEN=[YOUR_TELEGRAM_BOT_TOKEN_HERE]
```

#### Notion
```bash
# Create integration: https://www.notion.so/my-integrations
NOTION_API_KEY=[YOUR_NOTION_API_KEY_HERE]
```

#### Additional (Commented Out)
```bash
# CLOUDFLARE_ACCOUNT_ID=[YOUR_CLOUDFLARE_ACCOUNT_ID_HERE]
# CLOUDFLARE_API_TOKEN=[YOUR_CLOUDFLARE_API_TOKEN_HERE]
# HF_TOKEN=[YOUR_HUGGINGFACE_TOKEN_HERE]
# WANDB_API_KEY=[YOUR_WANDB_API_KEY_HERE]
```

### Logging

```bash
LOG_LEVEL=info
```

---

## 🏠 Hermes Directory Structure

Hermes v0.16.0 stores its configuration under `~/.hermes/`:

```
~/.hermes/
├── config.yaml            # Primary config file
├── profiles/              # Profile system
│   └── default/           # Your active profile
│       ├── skills/        # Profile-specific skills
│       ├── memories/      # Profile-specific memory
│       └── cron/          # Profile-specific cron jobs
├── skills/                # Global skills (50+ built-in)
├── memories/              # Global memory store
└── logs/                  # Log files
```

### config.yaml

The primary config file is `~/.hermes/config.yaml`. You rarely need to edit it directly — use `hermes setup` for initial configuration.

Key sections:
```yaml
# Provider configuration
provider:
  ollama-cloud:
    api_key: "${OLLAMA_API_KEY}"   # Reads from .env
    default_model: "deepseek-v4-flash"

# Gateway configuration (for Telegram, etc.)
gateway:
  telegram:
    enabled: true
    bot_token: "${TELEGRAM_BOT_TOKEN}"

# Profile settings
profile:
  active: "default"
  auto_load_skills: true
```

---

## 🔐 Hermes Vault

Hermes v0.16.0 includes a vault system for securely storing secrets:

```bash
# Store a secret in the vault
hermes vault set "my-secret-name" "my-secret-value"

# Retrieve a secret
hermes vault get "my-secret-name"

# List vault keys
hermes vault list
```

The vault encrypts secrets at rest. Use this instead of plaintext in config files.

---

## 🔄 Profiles System

Hermes supports multiple profiles for different environments:

```bash
# List profiles
hermes profile list

# Create a new profile
hermes profile create "work"
hermes profile create "personal"

# Switch profile
hermes profile switch "work"

# View active profile
hermes profile current
```

Each profile has its own `skills/`, `memories/`, `cron/` directories under `~/.hermes/profiles/<name>/`.

---

## 🚪 Gateway Configuration

The gateway routes messages between platforms and Hermes:

```yaml
# In config.yaml (set via wizard or manually)
gateway:
  telegram:
    enabled: true
    bot_token: "${TELEGRAM_BOT_TOKEN}"
    polling_interval: 2  # seconds
```

For now, Telegram is the primary gateway. Support for Discord and Slack is planned.

---

## ✅ Configuration Checklist

- [ ] `.env` file created with your credentials (placeholders replaced)
- [ ] GitHub token has `repo` scope
- [ ] Ollama Cloud API key configured
- [ ] Telegram bot created and tested (optional)
- [ ] `hermes setup` completed
- [ ] Vault initialized for secrets
- [ ] Profile configured (if using multi-environment)
- [ ] `.env` added to `.gitignore` ✅

---

## 🧪 Test Configuration

```bash
# Test GitHub connectivity
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user | grep login

# Test Ollama Cloud connectivity
curl -H "Authorization: Bearer [YOUR_OLLAMA_API_KEY]" \
  https://api.ollama.com/api/tags

# Test Hermes status
hermes setup --status
```

---

## 🎯 Next Steps

[→ First Agent](04-first-agent.md)