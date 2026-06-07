# 06 — Model Routing

Intelligent model selection for optimal AI performance with Hermes Agent v0.16.0.

---

## 🎯 What is Model Routing?

Model routing automatically selects the best AI model for each task based on:
- **Task type** (coding, reasoning, quick chat)
- **Complexity** (simple vs. deep analysis)
- **Speed requirements** (fast vs. thorough)

Hermes v0.16.0 uses **Ollama Cloud** as the primary provider with **deepseek-v4-flash** as the default model.

---

## 📊 Available Models (41+ on Ollama Cloud)

### Default Model

| Model | Provider | Best For |
|-------|----------|----------|
| `deepseek-v4-flash` | Ollama Cloud | General purpose (balanced speed/quality) |

### Available Models on Ollama Cloud

Ollama Cloud provides 41+ models. Key categories:

#### Reasoning (Advanced)
| Model | Strengths |
|-------|-----------|
| `deepseek-v4-flash` | Fast, general reasoning |
| `deepseek-r1` | Deep reasoning, complex analysis |
| `deepseek-r1-distill-llama-70b` | High-quality distilled reasoning |
| `qwq-32b` | Quick question answering |

#### Coding
| Model | Strengths |
|-------|-----------|
| `deepseek-v4-flash` | General coding tasks |
| `qwen2.5-coder-32b` | Inline code generation |
| `codegeex4` | Code completion and generation |

#### General Purpose
| Model | Strengths |
|-------|-----------|
| `llama_3_3-70b` | Versatile chat and analysis |
| `qwen-2.5-72b` | Multi-language support |
| `nemotron-70b` | Instruction following |
| `mistral-small-24b` | Lightweight, fast responses |

#### Vision / Multimodal
| Model | Strengths |
|-------|-----------|
| `llama_3_2_11b-vision-instruct-fp16` | Image analysis |
| `llama_3_2_90b-vision-instruct-fp16` | Advanced vision tasks |
| `minicpm-v2_6` | Vision + language |

> **Full list:** Visit [ollama.com/models](https://ollama.com/models) for the complete catalog.

---

## ⚙️ Provider Configuration

### Ollama Cloud (Default)

```bash
# In .env
OLLAMA_API_KEY=[YOUR_OLLAMA_API_KEY_HERE]
```

Get your API key from: https://ollama.com/settings/keys

### Ollama Local (Alternative)

If you prefer to run models locally:

```bash
# In .env (uncomment to override cloud provider)
# OLLAMA_BASE_URL=http://localhost:11434/v1
# WARNING: This overrides the ollama-cloud provider!
```

Local Ollama requires:
- Running `ollama serve` locally
- Models pulled locally via `ollama pull <model>`
- Sufficient hardware (varies by model)

---

## 🎛️ Model Selection in Practice

### Default: deepseek-v4-flash

The default model `deepseek-v4-flash` is suitable for most tasks:

```bash
hermes run "analyze this Python code for bugs"
# Uses: deepseek-v4-flash
```

### Per-Task Routing with delegate_task

When building skills, you can specify the model per task:

```python
# In a skill
result = delegate_task(
    task="write a complex algorithm",
    model="deepseek-v4-flash",  # or any model from Ollama Cloud
    timeout=300
)
```

### CLI Override

```bash
# Force a different model
hermes run "analyze this image" --model "llama_3_2_90b-vision-instruct-fp16"
```

---

## 🧪 Testing Models

### List Available Models via Ollama API

```bash
curl -H "Authorization: Bearer [YOUR_OLLAMA_API_KEY]" \
  https://api.ollama.com/api/tags
```

### Quick Model Test

```bash
hermes run "say hello" --model "deepseek-v4-flash"
hermes run "say hello" --model "mistral-small-24b"
```

---

## 🎯 Model Selection Guide

| Task Type | Recommended Model |
|-----------|------------------|
| Quick Q&A | `deepseek-v4-flash` |
| Coding | `deepseek-v4-flash` |
| Code generation | `qwen2.5-coder-32b` |
| Analysis/Reasoning | `deepseek-v4-flash` |
| Vision tasks | `llama_3_2_90b-vision-instruct-fp16` |
| Fast responses | `mistral-small-24b` |
| Balanced | `deepseek-v4-flash` |

---

## 📈 Per-Task Routing in delegate_task

For advanced workflows, you can route different sub-tasks to different models:

```python
# Multi-step workflow with model routing
research = delegate_task(
    task="research the topic",
    model="qwq-32b",  # fast for research
    timeout=120
)

analysis = delegate_task(
    task=f"analyze this research: {research}",
    model="deepseek-v4-flash",  # deep for analysis
    timeout=300
)

summary = delegate_task(
    task=f"summarize: {analysis}",
    model="mistral-small-24b",  # lightweight for summary
    timeout=60
)
```

---

## 🔧 Model Router Skill

Hermes v0.16.0 has a built-in `model-router` skill:

```bash
# View the skill
hermes run "show me the model-router skill documentation"

# Use auto-routing
hermes run "fix this bug" --skill "model-router"
# Automatically selects the best model for debugging
```

---

## ✅ Model Routing Checklist

- [ ] Ollama Cloud API key configured in `.env`
- [ ] Can call the default model (`deepseek-v4-flash`)
- [ ] Can override model with `--model` flag
- [ ] Understands per-task routing with `delegate_task`
- [ ] Knows how to list available models

---

## 🚀 Next Steps

[→ Advanced Topics](07-advanced.md)