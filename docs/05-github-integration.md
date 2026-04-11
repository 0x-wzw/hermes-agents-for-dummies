# 05 - GitHub Integration

Complete guide to connecting Hermes with GitHub.

---

## 🔐 Creating a Personal Access Token

### Step 1: Generate Token

1. Go to: https://github.com/settings/tokens
2. Click **"Generate new token (classic)"**
3. **Note**: `hermes-agent`
4. **Expiration**: 90 days (recommended)
5. **Scopes** to select:
   - ✅ `repo` (Full control of private repositories)
   - ✅ `workflow` (Update GitHub Action workflows)
   - ✅ `read:org` (Read org and team membership)
6. Click **Generate token**
7. **COPY THE TOKEN** (you won't see it again!)

### Step 2: Store Securely

```bash
# Option 1: git credentials store
git config --global credential.helper store
echo "https://0x-wzw:[YOUR_TOKEN_HERE]@github.com" > ~/.git-credentials
chmod 600 ~/.git-credentials

# Option 2: Environment variable
export GITHUB_TOKEN=[YOUR_TOKEN_HERE]
echo 'export GITHUB_TOKEN=[YOUR_TOKEN_HERE]' >> ~/.bashrc
```

---

## 🔄 Testing Connection

```bash
# Test with curl
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/user

# Expected: JSON with your profile info
```

---

## 📋 Common GitHub Tasks

### List Your Repositories

```bash
# View all repos
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/user/repos?type=all&per_page=100

# Filter by private/public
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/user/repos?type=private
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/user/repos?type=public
```

### Clone a Repository

```bash
# Via HTTPS (uses stored credentials)
git clone https://github.com/0x-wzw/repo-name.git

# Via SSH (if SSH key configured)
git clone git@github.com:0x-wzw/repo-name.git
```

### Create a New Repository

```bash
curl -X POST \
  -H "Authorization: token [YOUR_TOKEN_HERE]" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/user/repos \
  -d '{
    "name": "my-new-repo",
    "description": "Created by Hermes",
    "private": false
  }'

# Returns: URL of new repo
```

---

## 🤖 Hermes + GitHub Workflows

### Example 1: Code Review Agent

```bash
# Spawn agent to review PR
curl -X POST http://localhost:9001/agent/spawn \
  -H "Content-Type: application/json" \
  -d '{
    "task": "review pull request https://github.com/0x-wzw/repo/pull/123",
    "model": "deepseek-v3.1:671b",
    "toolsets": ["terminal", "github"],
    "deliver": "telegram"
  }'

# Agent will:
# 1. Fetch PR diff
# 2. Analyze code
# 3. Post review comments
# 4. Notify you via Telegram
```

### Example 2: Automated Documentation

```bash
# Update README based on code changes
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "review recent commits in Capital-Sentience and update README",
    "model": "qwen3-coder:480b",
    "toolsets": ["terminal", "file", "github"],
    "skills": ["github-pr-workflow"]
  }'
```

### Example 3: Repository Cleanup

```bash
# Consolidate multiple repos
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "consolidate these repos: repo1, repo2, repo3 into unified-repo",
    "toolsets": ["terminal", "file", "github"],
    "skills": ["github-repo-management"]
  }'
```

---

## 📊 GitHub Skills Available

Hermes has built-in skills for GitHub operations:

| Skill | Purpose | Command |
|-------|---------|---------|
| `github-auth` | Authentication setup | `skill_view(github-auth)` |
| `github-pr-workflow` | PR create/review/merge | View via skill_view |
| `github-code-review` | Automated code review | `delegate_task(...)` |
| `github-issues` | Issue management | Built-in tool |
| `github-repo-management` | Clone/fork/config | Built-in tool |

---

## 🔧 Troubleshooting

### "401 Bad Credentials"
```bash
# Token expired or invalid
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/user

# If 401, regenerate token at:
# https://github.com/settings/tokens
```

### "403 Rate Limit Exceeded"
```bash
# Check rate limit
curl -H "Authorization: token [YOUR_TOKEN_HERE]" \
  https://api.github.com/rate_limit

# Authenticated: 5000 requests/hour
# Unauthenticated: 60 requests/hour
```

### "Repository not found"
```bash
# Check token has 'repo' scope
# Verify repository name/owner
# Check if private repo + token permissions
```

---

## 🎯 Best Practices

### 1. Token Security
- **Never** commit tokens to git
- Use environment variables or credential helper
- Set expiration dates
- Revoke unused tokens

### 2. Scope Management
- Only request scopes you need
- Use fine-grained tokens when possible
- Separate tokens for different environments

### 3. Automation
- Use `cronjob` for scheduled GitHub tasks
- Store backup of critical repos
- Monitor rate limits

---

## 📝 Example: Full GitHub Workflow

```bash
# 1. Set up authentication
export GITHUB_TOKEN=[YOUR_TOKEN_HERE]

# 2. Create repo structure
curl -X POST ... (create repo)

# 3. Clone locally
git clone https://github.com/0x-wzw/new-repo.git

# 4. Add files
cd new-repo
echo "# My Project" > README.md

# 5. Push via Hermes agent
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "commit README.md and push to main",
    "toolsets": ["terminal"],
    "workdir": "/path/to/new-repo"
  }'

# 6. Create PR for new feature
curl -X POST http://localhost:9001/agent/spawn \
  -d '{
    "task": "create branch feature-x, add changes, open PR",
    "skills": ["github-pr-workflow"]
  }'
```

---

## ✅ GitHub Integration Checklist

- [ ] Personal Access Token created
- [ ] Token has `repo` scope
- [ ] Token stored securely (not in git)
- [ ] Can list repositories
- [ ] Can clone repositories
- [ ] Can spawn agent with GitHub tasks
- [ ] Can receive PR notifications

---

## 🚀 Next: Intelligent Model Routing

[→ Model Routing](06-model-routing.md)
