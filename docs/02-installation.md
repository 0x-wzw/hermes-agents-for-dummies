# 02 - Installation

This guide walks you through installing Hermes from scratch.

---

## 🚀 Choose Your Method

| Method | Difficulty | Time | Best For |
|--------|------------|------|----------|
| **Docker** 🐳 | Easy | 15 min | Most users |
| **Source** 🛠️ | Medium | 30 min | Developers |
| **Docker Compose** 📦 | Easy | 10 min | Production-ready |

---

## 🐳 Method 1: Docker (Recommended)

### Step 1: Pull Hermes Image

```bash
# Pull latest Hermes image
docker pull hermes/ai-agent:latest

# Verify
docker images | grep hermes
```

### Step 2: Create Environment

```bash
# Create project directory
mkdir ~/hermes-project
cd ~/hermes-project

# Copy environment template
curl -o .env.example https://raw.githubusercontent.com/0x-wzw/hermes-agents-for-dummies/main/.env.example

# Create your .env file
cp .env.example .env

# Edit with your values (see Config section)
nano .env  # or vim, code, etc.
```

### Step 3: Run Hermes

```bash
# Run with docker
docker run -d \
  --name hermes-agent \
  -p 9001:9001 \
  -v $(pwd)/.env:/app/.env \
  -v $(pwd)/data:/app/data \
  hermes/ai-agent:latest

# Check logs
docker logs -f hermes-agent
```

### Step 4: Verify Installation

```bash
# Test API is running
curl http://localhost:9001/health

# Expected response: {"status": "ok"}
```

✅ **Docker installation complete!**

---

## 🛠️ Method 2: Source Installation

### Step 1: Clone Repository

```bash
# Create workspace
mkdir ~/projects
cd ~/projects

# Clone Hermes source
git clone https://github.com/hermes-ai/agents.git
cd agents

# (Alternative: Download specific release)
git checkout v2.5.0  # Latest stable
```

### Step 2: Create Virtual Environment

```bash
# Create venv
python3 -m venv hermes-venv

# Activate
source hermes-venv/bin/activate

# Verify
which python
# Should show: ~/projects/agents/hermes-venv/bin/python
```

### Step 3: Install Dependencies

```bash
# Upgrade pip
pip install --upgrade pip

# Install core
pip install -r requirements.txt

# Install dev tools (optional)
pip install -r requirements-dev.txt

# Verify
pip list | grep -E "hermes|openai|requests"
```

### Step 4: Configuration

```bash
# Create config directory
mkdir -p ~/.hermes/config

# Copy templates
cp config/example.config.yaml ~/.hermes/config/config.yaml

# Copy environment
cp .env.example ~/.hermes/.env

# Edit configuration
nano ~/.hermes/config/config.yaml
nano ~/.hermes/.env
```

### Step 5: Initialize Hermes

```bash
# Run setup
python -m hermes init

# Test
curl http://localhost:9001/health
```

✅ **Source installation complete!**

---

## 📦 Method 3: Docker Compose (Production)

### Step 1: Create Compose File

```yaml
# docker-compose.yaml
version: '3.8'

services:
  hermes:
    image: hermes/ai-agent:latest
    container_name: hermes-agent
    restart: unless-stopped
    ports:
      - "9001:9001"
    volumes:
      - ./.env:/app/.env:ro
      - ./data:/app/data
      - ./skills:/app/skills
    environment:
      - HERMES_LOG_LEVEL=info
    networks:
      - hermes-network

  # Optional: Redis for caching
  redis:
    image: redis:7-alpine
    container_name: hermes-redis
    restart: unless-stopped
    volumes:
      - redis-data:/data
    networks:
      - hermes-network

  # Optional: PostgreSQL for persistence
  postgres:
    image: postgres:15-alpine
    container_name: hermes-postgres
    restart: unless-stopped
    environment:
      POSTGRES_USER: hermes
      POSTGRES_PASSWORD: [YOUR_DB_PASSWORD_HERE]
      POSTGRES_DB: hermes
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - hermes-network

volumes:
  redis-data:
  postgres-data:

networks:
  hermes-network:
    driver: bridge
```

### Step 2: Start Services

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f hermes

# Stop
docker-compose down

# Stop and remove volumes (clean slate)
docker-compose down -v
```

✅ **Docker Compose installation complete!**

---

## 🔍 Post-Installation Verification

Run this comprehensive check:

```bash
#!/bin/bash
# save as: verify-installation.sh

echo "🔍 Hermes Installation Verification"
echo "==================================="

# Check process
if docker ps | grep -q hermes; then
    echo "✅ Hermes container running"
else
    echo "❌ Hermes container not running"
fi

# Check API
RESPONSE=$(curl -s http://localhost:9001/health 2>/dev/null)
if [ "$RESPONSE" = '{"status": "ok"}' ]; then
    echo "✅ Health endpoint responding"
else
    echo "❌ Health check failed"
fi

# Check logs
if docker logs hermes-agent 2>/dev/null | grep -q "ready"; then
    echo "✅ Agent initialized"
else
    echo "⚠️  Check logs: docker logs hermes-agent"
fi

# Check disk usage
USAGE=$(docker system df | grep hermes 2>/dev/null || echo "N/A")
echo "ℹ️  Docker usage: $USAGE"

echo "==================================="
echo "Verification complete!"
```

Run: `chmod +x verify-installation.sh && ./verify-installation.sh`

---

## 📂 Understanding the File Structure

```
~/hermes-project/
├── .env                    # Your credentials (NEVER commit)
├── .env.example            # Template
├── docker-compose.yaml     # Full stack
├── data/                   # Persistent data
│   ├── logs/
│   ├── memory/
│   └── skills/
├── skills/                 # Custom skills
└── backups/                # Automated backups
```

---

## 🎯 First Agent Spawn Test

```bash
# Basic test
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "say hello",
    "model": "ministral-3:8b",
    "timeout": 60
  }'

# Expected: JSON response with agent output
```

---

## 🚨 Troubleshooting

### "Port 9001 already in use"
```bash
# Find process
sudo lsof -i :9001

# Kill if needed
sudo kill -9 <PID>

# Or map to different port
docker run -p 9002:9001 ...
```

### "Permission denied"
```bash
# Fix data directory permissions
chmod 755 ~/hermes-project/data
chown -R $USER:$USER ~/hermes-project
```

### "Model not found"
```bash
# Verify Ollama connectivity
curl http://localhost:11434/api/tags

# Pull specific model
curl -X POST http://localhost:11434/api/pull \
  -d '{"name": "ministral-3:8b"}'
```

---

## ✅ Installation Complete?

If all checks pass:
- [ ] Hermes container running
- [ ] Health endpoint responds
- [ ] Can spawn basic agent
- [ ] Logs show no critical errors

[→ Continue to Configuration](03-configuration.md)
