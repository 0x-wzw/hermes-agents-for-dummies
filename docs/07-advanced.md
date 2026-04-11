# 07 - Advanced Topics

Advanced configuration and operations for Hermes.

---

## 🔐 Security Best Practices

### Credential Management

```bash
# Never store in git
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Secure .env"

# Use environment-specific configs
# .env.development
# .env.production
# .env.local (gitignored)
```

### Docker Sandbox Security

```yaml
# ~/.hermes/config/sandbox-security.yaml
sandbox:
  security:
    # Run as non-root
    user: "1000:1000"
    
    # Read-only filesystem
    read_only: true
    
    # Drop all capabilities
    cap_drop:
      - ALL
    
    # No new privileges
    security_opt:
      - "no-new-privileges:true"
    
    # Network isolation
    network: "none"
    
    # Resource limits
    memory: "2g"
    cpus: "1.0"
    pids_limit: 100
    
    # Volume restrictions
    volumes:
      - "/app/data:/data:rw"
      - "/app/shared:/shared:ro"
```

---

## 🐳 Docker Sandbox Deep Dive

### Custom Docker Images

```dockerfile
# Dockerfile.custom-agent
FROM python:3.11-slim

# Install specific packages
RUN apt-get update && apt-get install -y \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python packages
RUN pip install requests pandas numpy

# Non-root user
RUN useradd -m -u 1000 agent
USER agent

WORKDIR /home/agent
```

### Build and Use

```bash
# Build custom image
docker build -f Dockerfile.custom-agent -t hermes-agent-custom .

# Configure to use
# ~/.hermes/config/sandbox.yaml
sandbox:
  image: "hermes-agent-custom"
  memory: "4g"
```

---

## 🔄 Multi-Agent Swarms

### Swarm Configuration

```yaml
# ~/.hermes/config/swarm.yaml
swarm:
  enabled: true
  max_agents: 10
  
  spawn_policy:
    timeout: 300
    max_retries: 3
    
  communication:
    protocol: "a2a"  # Agent-to-Agent
    shared_memory: true
    
  coordination:
    leader_election: true
    load_balancing: "round_robin"
    
  monitoring:
    health_checks: true
    metrics_interval: 30
```

### Swarm Example

```bash
# Spawn coordinated agents
curl -X POST http://localhost:9001/swarm/spawn \
  -d '{
    "agents": [
      {"task": "research competitors", "model": "kimi-k2.5"},
      {"task": "analyze tech stack", "model": "deepseek-v3.1:671b"},
      {"task": "write summary", "model": "ministral-3:8b"}
    ],
    "coordination": "sequential",
    "shared_context": true
  }'
```

---

## 📊 Monitoring & Logging

### Enable Detailed Logging

```bash
# Set log level
export HERMES_LOG_LEVEL=debug

# Log to file
export HERMES_LOG_FILE=/var/log/hermes/agent.log

# Structured logging (JSON)
export HERMES_LOG_FORMAT=json
```

### Health Monitoring

```bash
# Check system health
curl http://localhost:9001/health

# Detailed metrics
curl http://localhost:9001/metrics

# Agent-specific metrics
curl http://localhost:9001/agent/{id}/metrics
```

---

## 🔌 MCP (Model Context Protocol)

### What is MCP?

MCP enables AI models to:
- Access external tools
- Retrieve context dynamically
- Interact with other services

### MCP Servers

```yaml
# ~/.hermes/config/mcp.yaml
mcp:
  servers:
    - name: "tavily-search"
      command: "npx"
      args: ["-y", "@tavily/mcp-server"]
      env:
        TAVILY_API_KEY: [YOUR_TAVILY_KEY]
        
    - name: "filesystem"
      command: "npx"
      args: ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/data"]
      
    - name: "sqlite"
      command: "uvx"
      args: ["mcp-server-sqlite", "--db-path", "/home/user/data.db"]
```

---

## 💾 Backup Strategy

### Automated Backups

```bash
#!/bin/bash
# backup-hermes.sh

BACKUP_DIR="$HOME/.hermes/backups/$(date +%Y%m%d)"
mkdir -p "$BACKUP_DIR"

# Backup memory
cp -r ~/.hermes/memory "$BACKUP_DIR/"

# Backup skills
cp -r ~/.hermes/skills "$BACKUP_DIR/"

# Backup config
cp -r ~/.hermes/config "$BACKUP_DIR/"

# Git backup (optional)
cd "$BACKUP_DIR"
git init
git add .
git commit -m "Backup $(date)"

# Remote push
git remote add origin https://github.com/[USER]/hermes-backup.git
git push origin main

echo "Backup complete: $BACKUP_DIR"
```

---

## 🔄 CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/hermes-ci.yaml
name: Hermes CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Hermes
        run: |
          curl -sSL https://install.hermes.dev | bash
          hermes init
          
      - name: Run Tests
        run: |
          hermes test
          
      - name: Spawn Review Agent
        run: |
          hermes agent spawn \
            --task "review this PR" \
            --model "deepseek-v3.1:671b"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 🎯 Troubleshooting Advanced

### Low Memory Issues

```bash
# Check memory
docker stats --no-stream

# Reduce concurrent agents
# ~/.hermes/config/swarm.yaml
swarm:
  max_agents: 3  # Reduce from 10
```

### Model Timeout

```bash
# Increase timeout
# ~/.hermes/config/routing.yaml
routing:
  default_timeout: 600  # seconds
  
# Or per-request
curl -X POST http://localhost:9001/agent/spawn \
  -d '{"task": "...", "timeout": 1800}'
```

### Network Issues

```bash
# Test connectivity
curl -v https://api.ollama.com/v1/models

# Check DNS
nslookup api.ollama.com

# Proxy settings (if behind corporate)
export HTTP_PROXY=http://proxy.company.com:8080
export HTTPS_PROXY=http://proxy.company.com:8080
```

---

## ✅ Advanced Checklist

- [ ] Security hardening applied
- [ ] Custom Docker images built
- [ ] Swarm coordination configured
- [ ] Monitoring enabled
- [ ] MCP servers connected
- [ ] Backup automation set up
- [ ] CI/CD pipeline created

---

**You're now a Hermes expert!** 🎓

For questions: [GitHub Issues](https://github.com/0x-wzw/hermes-agents-for-dummies/issues)
