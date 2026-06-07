# 02 — Installation

This guide walks you through installing Hermes Agent v0.16.0 from Nous Research.

---

## 🚀 Choose Your Method

| Method | Difficulty | Time | Best For |
|--------|------------|------|----------|
| **pip** 🐍 | Easy | 5 min | Most users |
| **uv** ⚡ | Easy | 3 min | Fast, modern setups |
| **Source** 🛠️ | Medium | 10 min | Developers / contributors |

---

## ⚡ Method 1: pip (Recommended)

The simplest way to install Hermes Agent:

### Step 1: Install

```bash
# Install the latest release
pip install hermes-agent

# Or pin to v0.16.0 explicitly
pip install hermes-agent==0.16.0

# Verify installation
hermes --version
```

### Step 2: Create Environment File

```bash
# Create a project directory
mkdir ~/hermes-project
cd ~/hermes-project

# Copy the template from this repo
cp /path/to/hermes-agents-for-dummies/.env.example .env

# Edit with your placeholder values
nano .env
```

### Step 3: Run Setup Wizard

```bash
# Hermes will guide you through initial setup
hermes setup
```

The wizard will:
1. Check Python compatibility (3.11+)
2. Create `~/.hermes/` directory structure
3. Prompt for API keys (all optional)
4. Set up Git identity
5. Verify connectivity to services

### Step 4: Verify Installation

```bash
# Check Hermes is accessible
hermes --help

# View setup summary
hermes setup --status
```

✅ **pip installation complete!**

---

## ⚡ Method 2: uv (Fast Alternative)

[uv](https://github.com/astral-sh/uv) is a Rust-based Python package manager — significantly faster than pip.

### Step 1: Install uv

```bash
# Linux/macOS
curl -LsSf https://astral.sh/uv/install.sh | sh

# Or via pip
pip install uv
```

### Step 2: Install Hermes with uv

```bash
# Create a virtual environment and install Hermes in one step
uv venv
source .venv/bin/activate

# Install Hermes Agent
uv pip install hermes-agent

# Or install specific version
uv pip install hermes-agent==0.16.0

# Verify
hermes --version
```

### Step 3: Setup

```bash
cp /path/to/hermes-agents-for-dummies/.env.example .env
nano .env
hermes setup
```

✅ **uv installation complete!**

---

## 🛠️ Method 3: Source Installation

For developers who want the latest code or need to contribute:

### Step 1: Clone NousResearch Repository

```bash
# Create workspace
mkdir -p ~/projects
cd ~/projects

# Clone official Hermes Agent source
git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent

# Checkout the v0.16.0 release tag
git checkout v0.16.0
```

### Step 2: Create Virtual Environment

```bash
# Using uv (preferred)
uv venv
source .venv/bin/activate
uv pip install -e ".[dev]"

# Or using pip
python3 -m venv venv
source venv/bin/activate
pip install -e ".[dev]"
```

### Step 3: Configure

```bash
# Copy environment template
cp /path/to/hermes-agents-for-dummies/.env.example .env

# Run setup
python -m hermes setup
```

### Step 4: Verify

```bash
hermes --version
# Expected: 0.16.0 or higher
```

✅ **Source installation complete!**

---

## 📂 Understanding the File Structure

```
~/.hermes/                          # Hermes home directory
├── config.yaml                     # Primary configuration file
├── profiles/                       # Profile system (multi-environment)
│   └── default/                    # Default profile
│       ├── skills/                 # Skills for this profile
│       ├── config/                 # Profile-specific config
│       └── memories/               # Profile-specific memories
├── skills/                         # Built-in skills (50+)
├── memories/                       # Agent memory storage
└── logs/                           # Log output

~/hermes-project/                   # Your project directory
├── .env                            # Your credentials (NEVER commit)
└── .env.example                    # Template with placeholders
```

---

## 🔍 Post-Installation Verification

```bash
# Run basic health check
hermes --version

# Check configuration
hermes setup --status

# Test model connectivity (Ollama Cloud)
hermes run "ping" --dry-run
```

---

## ✅ Installation Complete?

- [ ] Hermes Agent installed (`hermes --version` works)
- [ ] Environment file created (`.env` with placeholders)
- [ ] Setup wizard completed (`hermes setup`)
- [ ] `~/.hermes/` directory structure exists

---

## 🚀 Next Steps

[→ Continue to Configuration](03-configuration.md)