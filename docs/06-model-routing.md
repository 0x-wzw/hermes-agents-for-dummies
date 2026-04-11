# 06 - Model Routing

Intelligent model selection for optimal AI performance.

---

## 🎯 What is Model Routing?

Model routing automatically selects the best AI model for each task based on:
- **Task type** (coding, reasoning, quick chat)
- **Complexity** (simple vs. deep analysis)
- **Speed requirements** (fast vs. thorough)
- **Cost considerations** (token usage)

---

## 📊 Available Models (38 Total)

### By Category

#### 🚀 Fast Models (< 10GB)
| Model | Size | Best For | Speed |
|-------|------|----------|-------|
| `ministral-3:3b` | 5GB | Quick Q&A | ⚡ Ultra-fast |
| `ministral-3:8b` | 10GB | General tasks | ⚡ Fast |
| `gemma3:4b` | 8GB | Lightweight work | ⚡ Ultra-fast |

#### 💻 Coding Specialists
| Model | Size | Strengths | Speed |
|-------|------|-----------|-------|
| `deepseek-v3.1:671b` | 688GB | Code review, debugging | 🐢 Thorough |
| `qwen3-coder:480b` | 510GB | Feature implementation | 🐢 Thorough |
| `qwen3-coder-next` | 82GB | Fast code gen | 🚀 Medium |
| `devstral-2:123b` | 128GB | Security audits | 🚀 Fast |

#### 🧠 Reasoning Champions
| Model | Size | Strengths | Speed |
|-------|------|-----------|-------|
| `kimi-k2-thinking` | 1.1TB | Deep analysis | 🐢 Slow |
| `cogito-2.1:671b` | 689GB | Philosophy/logic | 🐢 Thorough |
| `deepseek-v3.2` | 689GB | Complex reasoning | 🐢 Thorough |

#### 🎯 General Purpose
| Model | Size | Strengths | Speed |
|-------|------|-----------|-------|
| `kimi-k2.5` (default) | 1.1TB | Everything | ⚖️ Balanced |
| `kimi-k2:1t` | 1.1TB | General tasks | ⚖️ Balanced |
| `glm-5` | 756GB | Research/writing | ⚖️ Balanced |
| `qwen3.5:397b` | 397GB | Balanced perf | ⚖️ Balanced |

#### 🎨 Vision Models
| Model | Size | Strengths |
|-------|------|-----------|
| `qwen3-vl:235b-instruct` | 470GB | Image analysis |
| `qwen3-vl:235b` | 470GB | Vision + text |

---

## 🎛️ Routing Configuration

Create `~/.hermes/config/routing.yaml`:

```yaml
routing:
  # Default model
  default: "kimi-k2.5"
  
  # Task-specific routing
  tasks:
    code_review:
      model: "deepseek-v3.1:671b"
      max_iterations: 100
      toolsets: ["terminal", "file"]
      
    bug_fix:
      model: "qwen3-coder:480b"
      reasoning: true
      
    quick_answer:
      model: "ministral-3:8b"
      timeout: 30
      
    complex_analysis:
      model: "kimi-k2-thinking"
      timeout: 300
      
    security_audit:
      model: "devstral-2:123b"
      toolsets: ["terminal", "security"]
      
    documentation:
      model: "ministral-3:8b"
      max_tokens: 2000

  # Fallback chain
  fallback:
    - "kimi-k2.5"
    - "kimi-k2:1t"
    - "ministral-3:14b"

  # Cost optimization
  cost_optimization:
    enabled: true
    max_cost_per_request: 0.10
    prefer_cheaper_if_under_95_percentile: true
```

---

## 💻 Usage Examples

### Automatic Routing (Recommended)

```bash
# Let system choose best model
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "fix this bug",
    "auto_route": true
  }'

# Routes to: deepseek-v3.1:671b (detected code task)
```

### Explicit Model Selection

```bash
# Force specific model
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "analyze trade-offs",
    "model": "kimi-k2-thinking"
  }'
```

### Cost-Conscious Routing

```bash
# Use cheaper models when possible
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "summarize this article",
    "max_cost": 0.01,
    "auto_route": true
  }'

# May route to ministral-3:8b instead of kimi-k2.5
```

---

## 🔧 Testing Models

### List Available Models

```bash
curl http://localhost:11434/api/tags | jq '.models[].name'

# Or via Hermes
curl http://localhost:9001/models/list
```

### Quick Model Test

```bash
#!/bin/bash
# test-models.sh

MODELS=("ministral-3:8b" "deepseek-v3.1:671b" "kimi-k2.5")

for MODEL in "${MODELS[@]}"; do
  echo "Testing: $MODEL"
  time curl -X POST http://localhost:9001/agent/spawn \
    -d "{\"task\": \"say hello\", \"model\": \"$MODEL\", \"timeout\": 60}"
  echo "---"
done
```

---

## 🎯 Routing Best Practices

### 1. Task Complexity Matching
```
Simple tasks → Fast models (ministral-3:8b)
Coding tasks → Coding specialists (deepseek, qwen3-coder)
Analysis → Reasoning models (kimi-k2-thinking)
Vision tasks → Multimodal (qwen3-vl)
```

### 2. Speed vs. Quality Trade-offs
```
Quick answers → Use fast models
tCritical decisions → Use thorough models
Iterative development → Balance based on phase
```

### 3. Cost Optimization
```yaml
# Enable cost tracking
cost_tracking:
  enabled: true
  log_usage: true
  
# Set budget constraints
budget:
  daily_limit: 5.00
  per_task_limit: 0.50
```

---

## 📈 Performance Metrics

### Track Model Performance

```bash
# Get routing statistics
curl http://localhost:9001/routing/stats

# Response:
# {
#   "total_requests": 1000,
#   "model_usage": {
#     "ministral-3:8b": 450,
#     "deepseek-v3.1:671b": 200,
#     "kimi-k2.5": 350
#   },
#   "avg_latency_ms": 2345,
#   "cost_savings_percent": 35
# }
```

---

## 🚀 Advanced: Custom Routing Rules

### Create Custom Router

```python
# custom_router.py

def route_task(task_description: str, context: dict) -> str:
    """Custom routing logic."""
    
    task_lower = task_description.lower()
    
    # Security tasks
    if any(word in task_lower for word in ["security", "vulnerability", "audit"]):
        return "devstral-2:123b"
    
    # Math-heavy
    if any(word in task_lower for word in ["calculate", "math", "formula"]):
        return "glm-5.1"
    
    # Default to auto-route
    return None  # Falls back to system router
```

---

## ✅ Model Routing Checklist

- [ ] Know available models in your system
- [ ] Configure routing.yaml
- [ ] Test automatic routing
- [ ] Set up cost tracking
- [ ] Test fallback behavior
- [ ] Create custom rules if needed

---

## 🎓 Next Steps

[→ Advanced Topics](07-advanced.md)
