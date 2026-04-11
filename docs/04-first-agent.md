# 04 - First Agent Spawn

Your first autonomous agent in 5 minutes.

---

## 🎯 Goal

Spawn a simple agent that says "Hello World" and understand the spawn process.

---

## 📝 Syntax

```bash
hermes agent spawn [options]
```

Or via API:
```bash
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '[payload]'
```

---

## 🚀 Quick Start Examples

### Example 1: Basic Spawn

```bash
# Spawn agent with default settings
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "say hello world",
    "timeout": 60
  }'

# Response:
# {
#   "agent_id": "ag_123456789",
#   "status": "running",
#   "model": "kimi-k2.5",
#   "output": "Hello World!",
#   "duration_ms": 1234
# }
```

### Example 2: With Specific Model

```bash
# Use lightweight model for simple task
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "explain what is blockchain",
    "model": "ministral-3:8b",
    "timeout": 120
  }'

# Response: Detailed explanation using ministral (faster)
```

### Example 3: Code Generation

```bash
# Use coding-specialized model
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "write a python function to calculate fibonacci",
    "model": "deepseek-v3.1:671b",
    "timeout": 300,
    "toolsets": ["terminal", "file"]
  }'

# Response: Python code + file creation
```

### Example 4: Multi-Step Task

```bash
# Agent with multiple steps
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "search for ERC-8004 token standard, then summarize key features",
    "model": "kimi-k2-thinking",
    "toolsets": ["terminal", "web"],
    "timeout": 180
  }'
```

---

## 📦 Spawn Parameters

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `task` | string | What you want the agent to do |

### Optional Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `model` | string | kimi-k2.5 | Which AI model to use |
| `timeout` | int | 60 | Max seconds to run |
| `toolsets` | array | [] | Tools to enable |
| `max_iterations` | int | 50 | Max tool calls |
| `deliver` | string | "origin" | Where to send result |
| `skills` | array | [] | Skills to load |

### Toolsets Available

```json
["terminal", "file", "web", "image", "cronjob"]
```

---

## 🎨 Spawn Patterns

### Pattern 1: Fire and Forget

```bash
# Spawn without waiting for result
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "analyze codebase and email summary",
    "model": "deepseek-v3.1:671b",
    "deliver": "telegram"
  }'

# Agent runs in background, results sent to Telegram
```

### Pattern 2: Batch Queue

```bash
# Multiple agents in parallel
curl -X POST http://localhost:9001/agent/spawn/batch \
  -H "Content-Type: application/json" \
  -d '{
    "tasks": [
      {"task": "write api docs", "model": "qwen3-coder:480b"},
      {"task": "write unit tests", "model": "deepseek-v3.1:671b"},
      {"task": "optimize imports", "model": "ministral-3:14b"}
    ]
  }'
```

### Pattern 3: Scheduled Agent (Cron)

```bash
# Recurring agent via cronjob
curl -X POST http://localhost:9001/cronjob/create \
  -H "Content-Type: application/json" \
  -d '{
    "name": "daily-report",
    "schedule": "0 9 * * *",
    "prompt": "generate daily crypto market summary",
    "model": "kimi-k2.5",
    "deliver": "email"
  }'
```

### Pattern 4: Delegated Subagent

```bash
# One agent delegates to multiple subagents
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "split code review into modules and delegate",
    "model": "kimi-k2-thinking",
    "delegate": true,
    "subagents": [
      {"task": "review security", "model": "devstral-2:123b"},
      {"task": "review performance", "model": "deepseek-v3.1:671b"}
    ]
  }'
```

---

## 🐛 Debugging Failed Spawns

### Check Status

```bash
# Get agent status
curl http://localhost:9001/agent/{agent_id}/status

# Response:
# {
#   "agent_id": "ag_123456",
#   "status": "failed",
#   "error": "Model not found: invalid-model",
#   "logs": ["..."]
# }
```

### View Logs

```bash
# All logs
docker logs hermes-agent

# Specific agent
curl http://localhost:9001/agent/{agent_id}/logs

# Tail real-time
docker logs -f hermes-agent | grep "agent_id"
```

### Common Errors

| Error | Solution |
|-------|----------|
| `Model not found` | Check available models: `curl http://localhost:11434/api/tags` |
| `Timeout exceeded` | Increase timeout parameter |
| `Permission denied` | Check file permissions or run without sandbox |
| `Tool not available` | Ensure toolset enabled in spawn request |

---

## 🎯 Model Selection Guide

### Quick Tasks (< 1min)
```json

{
    "task": "task",
    "model": "ministral-3:8b",
    "timeout": 60
}
```

### Coding Tasks
```json

{
    "task": "task",
    "model": "qwen3-coder:480b",
    "timeout": 60
}
```

### Complex Analysis
```json

{
    "task": "task",
    "model": "kimi-k2-thinking",
    "timeout": 60
}
```

### Default (Balanced)
```json

{
    "task": "task",
    "model": "kimi-k2.5",
    "timeout": 60
}
```

---

## 🎁 Bonus: Telegram Notifications

```bash
# Spawn and get notified on completion
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "research Ethereum 2025 roadmap",
    "model": "kimi-k2-thinking",
    "deliver": "telegram"
  }'

# You'll get message: "Agent ag_xxx complete: {summary}"
```

---

## ✅ First Agent Checklist

- [ ] Can spawn basic agent
- [ ] Can specify model
- [ ] Can enable toolsets
- [ ] Can set timeout
- [ ] Can view logs on failure
- [ ] Can receive notifications

---

## 🚀 Next: Multi-Agent Swarms

[→ GitHub Integration](05-github-integration.md)
