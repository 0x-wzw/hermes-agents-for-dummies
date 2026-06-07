# 05 — GitHub Integration

Complete guide to connecting Hermes Agent v0.16.0 with GitHub.

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
7. **COPY THE TOKEN** — you won't see it again!

### Step 2: Store in `.env`

```bash
# Add to your .env file
GITHUB_TOKEN=[YOUR_GITHUB_TOKEN_HERE]
```

---

## 🔄 Alternative: GitHub CLI (gh)

The `gh` CLI is an alternative to managing tokens manually:

```bash
# Install gh (Ubuntu/Debian)
sudo apt install gh

# macOS
brew install gh

# Authenticate (stores token securely)
gh auth login

# Token is stored in the system keychain
# gh handles token refresh automatically
```

---

## 🔄 Backup Remote Pattern

For public repos like this one, use a **two-remote approach**:

```bash
# Public repo (documentation only — NO credentials)
git remote add origin https://github.com/[YOUR_USERNAME]/hermes-agents-for-dummies.git

# Private backup repo (full configs with real values)
git remote add backup https://github.com/[YOUR_USERNAME]/hermes-agents-for-dummies-backup.git

# Push public to origin
git push origin main

# Push private configs to backup
git push backup main
```

---

## 🧪 Testing Connection

```bash
# Test with curl
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user

# Expected: JSON with your profile info
```

> **Note:** Hermes v0.16.0 uses `Authorization: Bearer` (not `token`).

---

## 📋 Common GitHub Tasks

### List Your Repositories

```bash
# View all repos
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user/repos?type=all&per_page=100
```

### Clone a Repository

```bash
# Via HTTPS
git clone https://github.com/[YOUR_USERNAME]/repo-name.git

# Via SSH (if SSH key configured)
git clone git@github.com:[YOUR_USERNAME]/repo-name.git
```

### Create a New Repository

```bash
curl -X POST \
  -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/user/repos \
  -d '{
    "name": "my-new-repo",
    "description": "Created by Hermes",
    "private": false
  }'
```

---

## 🤖 Hermes + GitHub Workflows

### Private Repository Workflow

```bash
# 1. Set up git config
git config --global user.name "[YOUR_GIT_USERNAME]"
git config --global user.email "[YOUR_EMAIL_HERE]"

# 2. Clone with token
git clone https://[YOUR_USERNAME]:[YOUR_GITHUB_TOKEN]@github.com/[YOUR_USERNAME]/private-repo.git

# 3. Run Hermes agent on the repo
hermes run "analyze this repository's code quality"
```

### Code Review Agent

```bash
hermes run "review the latest pull request in my-repo"
```

### Automated Documentation

```bash
hermes run "review recent commits and update the README"
```

---

## 📊 GitHub Skills Available

Hermes has built-in skills for GitHub operations. Use `skill_view` to see details:

| Skill | Purpose |
|-------|---------|
| `github-auth` | Authentication setup |
| `github-pr-workflow` | PR create/review/merge |
| `github-code-review` | Automated code review |
| `github-issues` | Issue management |

```bash
# View a skill's documentation
hermes run "show me the github-auth skill documentation"
```

---

## 🔧 Troubleshooting

### "401 Bad Credentials"

```bash
# Test token
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/user

# If 401, regenerate token at:
# https://github.com/settings/tokens
```

### "403 Rate Limit Exceeded"

```bash
# Check rate limit
curl -H "Authorization: Bearer [YOUR_GITHUB_TOKEN]" \
  https://api.github.com/rate_limit

# Authenticated: 5000 requests/hour
# Unauthenticated: 60 requests/hour
```

### "Repository not found"

- Check token has `repo` scope
- Verify repository name/owner
- Check if private repo + token permissions

---

## 🎯 Best Practices

### 1. Token Security
- **Never** commit tokens to git (.gitignore protects .env)
- Use environment variables or credential helper
- Set expiration dates
- Revoke unused tokens
- Use private backup repo for real configs

### 2. Scope Management
- Only request scopes you need
- Use fine-grained tokens when possible
- Separate tokens for different environments

### 3. Public vs Private
- Public repo: documentation and placeholders only
- Private backup repo: real configurations and secrets
- Two-remote pattern: push docs to public, configs to private

---

## ✅ GitHub Integration Checklist

- [ ] Personal Access Token created
- [ ] Token has `repo` scope
- [ ] Token stored in `.env` (not in git)
- [ ] Can test connection with curl
- [ ] Git identity configured
- [ ] Private backup repo set up (optional)

---

## 🚀 Next Steps

[→ Model Routing](06-model-routing.md)