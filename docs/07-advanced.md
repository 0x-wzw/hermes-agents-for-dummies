# 07 — Advanced Topics

Advanced configuration and operations for Hermes Agent v0.16.0.

---

## 🔐 Security Best Practices

### Credential Management

```bash
# Never store in git — .gitignore protects .env
# Use Hermes Vault for secrets
hermes vault set "my-api-key" "sk-xxx..."
hermes vault get "my-api-key"
```

### Tirith Security

Hermes v0.16.0 includes **Tirith** — a built-in safety layer that:
- Validates tool calls before execution
- Prevents dangerous commands
- Sandboxes file operations
- Logs all agent actions for audit

```yaml
# In ~/.hermes/config.yaml
tirith:
  enabled: true
  rules:
    - "block-dangerous-commands"
    - "validate-file-paths"
    - "rate-limit-network"
  audit_log: "~/.hermes/logs/audit.log"
```

---

## 🧠 SOUL Identity

SOUL (System of Unified Logic) gives Hermes a persistent identity and personality:

```yaml
# In ~/.hermes/config.yaml
soul:
  identity: "hermes-agent"
  description: "Helpful AI assistant"
  constraints:
    - "Always be helpful and honest"
    - "Never reveal real credentials"
  memory:
    persist: true
    recall: true
```

The SOUL identity persists across sessions and is used for:
- Consistent agent behavior
- Memory recall between runs
- Personality and tone settings

---

## ⏰ Cron Jobs

Schedule recurring agent tasks:

```yaml
# ~/.hermes/profiles/default/cron/cron.yaml
cron:
  # Daily summary at 9 AM
  - name: "daily-report"
    schedule: "0 9 * * *"
    task: "generate daily market summary"
    model: "deepseek-v4-flash"
    deliver: "telegram"

  # Hourly check
  - name: "hourly-check"
    schedule: "0 * * * *"
    task: "check for system updates"
    timeout: 120

  # Weekly cleanup
  - name: "weekly-cleanup"
    schedule: "0 0 * * 0"
    task: "clean up temporary files and logs"
```

```bash
# List scheduled jobs
hermes cron list

# Test a cron job immediately
hermes cron run "daily-report"

# Disable a job
hermes cron disable "hourly-check"
```

---

## 📋 Kanban Dispatch

Organize agent tasks using kanban-style boards:

```yaml
# In cron.yaml or skill configuration
kanban:
  # Auto-dispatch based on task type
  dispatch:
    - type: "bug"
      skill: "debug-skill"
      model: "deepseek-v4-flash"
    - type: "feature"
      skill: "codegen-skill"
      model: "deepseek-v4-flash"
    - type: "documentation"
      skill: "docs-skill"
      model: "deepseek-v4-flash"
```

This integrates with GitHub issues:
- New `bug` label issues → auto-assigned to debug agent
- New `enhancement` label issues → auto-assigned to codegen agent
- New `documentation` label issues → auto-assigned to docs agent

---

## 🔄 Multi-Agent Coordination

Hermes v0.16.0 supports agent-to-agent communication:

```python
# Skill example: coordinate multiple agents
research_result = delegate_task(
    task="research competitors",
    model="deepseek-v4-flash",
    timeout=300
)

analysis = delegate_task(
    task=f"analyze this research: {research_result}",
    model="deepseek-v4-flash",
    timeout=300
)

summary = delegate_task(
    task=f"write summary: {analysis}",
    model="deepseek-v4-flash",
    timeout=120
)
```

---

## 💾 Persistence and Checkpointing

Hermes can persist agent state for long-running tasks:

```yaml
# In ~/.hermes/config.yaml
persistence:
  enabled: true
  checkpoint_interval: 60  # seconds
  storage: "local"          # Options: local, git
  backup_to_git: true       # Auto-push persisted state
```

### Checkpoint Example

```bash
# Run a long task with checkpointing
hermes run "analyze this large codebase"

# If interrupted, resume from last checkpoint
hermes run --resume
```

---

## 🖥️ Terminal Backend Configuration

Hermes v0.16.0 uses the terminal backend for code execution (not Docker sandboxes):

```yaml
# In ~/.hermes/config.yaml
terminal:
  work_dir: "/tmp/hermes-workdir"
  timeout: 300
  cleanup: true                   # Clean up after task
  allow_network: false            # Isolate from network
  max_output_lines: 1000
```

---

## 📊 Monitoring & Logging

### Enable Detailed Logging

```bash
# Set in .env
LOG_LEVEL=debug
```

### View Logs

```bash
# Tail logs
tail -f ~/.hermes/logs/hermes.log

# View last run
hermes status --verbose
```

---

## 🔌 MCP (Model Context Protocol)

Hermes v0.16.0 supports MCP for extending agent capabilities:

```yaml
# In ~/.hermes/config.yaml
mcp:
  servers:
    - name: "tavily-search"
      command: "npx"
      args: ["-y", "@tavily/mcp-server"]
      env:
        TAVILY_API_KEY: "${TAVILY_API_KEY}"
    
    - name: "filesystem"
      command: "npx"
      args: ["-y", "@modelcontextprotocol/server-filesystem", "/tmp/data"]
```

---

## 💾 Backup Strategy

### Automated Backups

```bash
#!/bin/bash
# backup-hermes.sh

BACKUP_DIR="$HOME/.hermes/backups/$(date +%Y%m%d)"
mkdir -p "$BACKUP_DIR"

# Backup memories
cp -r ~/.hermes/memories "$BACKUP_DIR/" 2>/dev/null

# Backup skills
cp -r ~/.hermes/skills "$BACKUP_DIR/" 2>/dev/null

# Backup config (exclude secrets)
cp ~/.hermes/config.yaml "$BACKUP_DIR/" 2>/dev/null

echo "Backup complete: $BACKUP_DIR"
```

---

## 🔄 CI/CD Integration

### GitHub Actions (for this repo)

```yaml
# .github/workflows/ci.yml
name: Hermes Docs CI

on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate markdown
        run: |
          echo "✅ Documentation validated"
```

---

## ✅ Advanced Checklist

- [ ] Tirith security enabled
- [ ] Cron jobs configured
- [ ] Kanban dispatch set up
- [ ] SOUL identity configured
- [ ] Persistence and checkpointing enabled
- [ ] Terminal backend configured
- [ ] Backup automation set up
- [ ] MCP servers connected (optional)

---

**You're now a Hermes expert!** 🎓

For questions: [GitHub Issues](https://github.com/0x-wzw/hermes-agents-for-dummies/issues)