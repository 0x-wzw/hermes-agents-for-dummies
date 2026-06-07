# 04 — First Agent

Your first autonomous agent in 5 minutes with Hermes Agent v0.16.0.

---

## 🎯 Goal

Run a simple agent that says "Hello World" and understand the workflow.

---

## 📝 CLI Syntax

```bash
hermes run "your prompt here"
```

---

## 🚀 Quick Start Examples

### Example 1: Basic Agent

```bash
# Run a simple task
hermes run "say hello world in one sentence"

# Output:
# Hello World! I'm ready to assist you.
```

### Example 2: Ask a Question

```bash
hermes run "what is the capital of France?"
```

### Example 3: Code Generation

```bash
hermes run "write a Python function to calculate fibonacci numbers"
```

### Example 4: Multi-Step Task (using delegate_task)

```bash
# Inside a skill or when scripting, you can use delegate_task
hermes run "search for recent Python news articles and summarize the top 3"
```

---

## 📦 Using delegate_task

`delegate_task` is the primary way to run agents programmatically (from skills or configuration):

```python
# In a Hermes skill
result = delegate_task(
    task="analyze this codebase for security issues",
    model="deepseek-v4-flash",  # default
    timeout=300
)
```

### Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `task` | string | (required) | What you want the agent to do |
| `model` | string | `deepseek-v4-flash` | Which AI model to use |
| `timeout` | int | 300 | Max seconds to run |
| `skills` | list | `[]` | Skills to load for this task |
| `context` | dict | `{}` | Additional context |

---

## 📦 Run Parameters

### CLI Parameters

| Flag | Description |
|------|-------------|
| `--model` | Override model (default: deepseek-v4-flash) |
| `--timeout` | Max seconds to run |
| `--profile` | Use a specific profile |
| `--dry-run` | Validate without executing |
| `--verbose` | Show detailed output |

### Examples

```bash
# Use a specific model
hermes run "explain quantum computing" --model "deepseek-v4-flash"

# Longer timeout
hermes run "write a comprehensive analysis" --timeout 600

# Use a specific profile
hermes run "check my calendar" --profile "work"
```

---

## 🎨 Workflow Patterns

### Pattern 1: Fire and Forget

```bash
# Run in background (notifications configured separately)
hermes run "analyze this repository and send me a summary" &
```

### Pattern 2: Sequential Tasks

```bash
# Chain multiple tasks
hermes run "first, find all TODO comments in the codebase"
hermes run "then, create GitHub issues for each TODO"
```

### Pattern 3: Scheduled Agent (Cron)

Scheduled tasks are configured through the cron system — see [Advanced Topics](07-advanced.md).

### Pattern 4: Delegated Subagent

```bash
# Use delegate_task to run sub-tasks
hermes run "split this code review into modules and delegate each module"
```

---

## 🐛 Debugging

### Check Status

```bash
# View last run status
hermes status

# View logs
tail -f ~/.hermes/logs/hermes.log
```

### Common Errors

| Error | Solution |
|-------|----------|
| `Model not found` | Check your Ollama Cloud API key in `.env` |
| `Timeout exceeded` | Increase `--timeout` parameter |
| `Connection refused` | Verify network and API key |
| `Authentication failed` | Check `GITHUB_TOKEN` in `.env` |

---

## 🎯 Model Selection

| Task Type | Recommended Model |
|-----------|------------------|
| Quick Q&A | `deepseek-v4-flash` (default) |
| Coding | `deepseek-v4-flash` |
| Analysis | `deepseek-v4-flash` |
| Complex reasoning | `deepseek-v4-flash` |

The default model `deepseek-v4-flash` is suitable for most tasks. See [Model Routing](06-model-routing.md) for advanced configuration.

---

## ✅ First Agent Checklist

- [ ] Can run basic agent: `hermes run "hello"`
- [ ] Can specify model with `--model`
- [ ] Can set timeout with `--timeout`
- [ ] Can view logs on failure
- [ ] Understands `delegate_task` pattern

---

## 🚀 Next Steps

[→ GitHub Integration](05-github-integration.md)