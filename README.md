# Automated Code Review Setup Guide

This guide walks you through setting up automated code reviews with Claude AI for your GitHub repositories.

## Prerequisites

1. **GitHub Account** with access to your repositories
2. **Claude API Key** from https://console.anthropic.com/account/keys
3. **Pro Membership** for Claude API (optional but recommended for cost efficiency)

## Setup Steps

### Step 1: Get Your Claude API Key

1. Go to https://console.anthropic.com/account/keys
2. Click "Create Key" or copy your existing key
3. Keep this key safe (don't commit to Git!)

### Step 2: Add API Key to GitHub

#### Option A: Organization-Level Secret (Recommended for Multiple Repos)

1. Go to your GitHub organization → **Settings**
2. Navigate to **Secrets and variables** → **Actions**
3. Click **New organization secret**
4. Name: `CLAUDE_API_KEY`
5. Value: (paste your API key)
6. Select which repos can access it (or all repos)

#### Option B: Repository-Level Secret (One Repo at a Time)

1. Go to your repository → **Settings**
2. Navigate to **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `CLAUDE_API_KEY`
5. Value: (paste your API key)

### Step 3: Add Workflow File to Your Repository

#### For Next.js Repositories:

1. Create directory structure:
   ```
   .github/workflows/
   ```

2. Create file: `.github/workflows/code-review-nextjs.yml`

3. Copy the entire contents from: `code-review-nextjs.yml`

4. Commit and push

#### For Django Repositories:

1. Create directory structure:
   ```
   .github/workflows/
   ```

2. Create file: `.github/workflows/code-review-django.yml`

3. Copy the entire contents from: `code-review-django.yml`

4. Commit and push

### Step 4: (Optional) Add tech stack specific Code Review Guidelines

Create a `.github/code-review-guidelines/` directory in your repo with:

- `custom.md` - Project-specific rules (if needed)

These are embedded in the workflow files by default, so this step is optional.

---

## How It Works

1. **PR Created/Updated**: When you create or update a PR against `dev` or `main`
2. **Workflow Triggers**: GitHub Actions automatically runs the code review
3. **Claude Reviews**: Claude API analyzes your code changes
4. **Comment Posted**: Results appear as a comment on your PR

Example comment:
```
## 🤖 Automated Code Review (Claude AI)

### 🔍 Issues Found
- **[HIGH]** N+1 query detected: User.objects.all() in loop. Location: views.py:45. Suggestion: Use select_related('profile')

### 💡 Suggestions & Improvements
- Consider extracting email validation logic to a separate function

### ✅ Positive Observations
- Good error handling in API endpoint
- Proper use of serializer validation

### 📋 Summary
Overall good code quality with one performance issue to address.
```

---

## Troubleshooting

### ❌ Workflow not triggering

- **Check 1**: PR is against `dev` or `main` branch?
- **Check 2**: YAML file is in `.github/workflows/` directory?
- **Check 3**: Filename ends in `.yml` or `.yaml`?
- **Check 4**: GitHub Actions is enabled in repo settings?

### ❌ "API key not found" error

- **Check 1**: Secret is named `CLAUDE_API_KEY` (case-sensitive)?
- **Check 2**: Secret exists in organization or repo settings?
- **Check 3**: Repository has access to the secret?
- **Check 4**: API key is valid (test in https://console.anthropic.com)

### ❌ Review comment not appearing

- **Check 1**: Workflow completed (check "Actions" tab)?
- **Check 2**: Look for errors in workflow logs
- **Check 3**: GitHub token has `pull-requests: write` permission (already set)

### ❌ Diff is truncated

- This happens if PR changes are > 50,000 characters
- Review will still work but may be incomplete
- Solution: Split large PRs into smaller ones

### ❌ Review takes too long

- Default timeout is 10 minutes
- If exceeded, increase `timeout-minutes` in workflow
- Large diffs (thousands of lines) take longer

---

## Customizing Reviews

### Change Review Criteria

Edit the workflow file (`.github/workflows/code-review-[tech].yml`):


### Only Review Certain File Types

Add a filter to the workflow:

```yaml
on:
  pull_request:
    branches:
      - main
      - dev
    paths:
      - '**.py'  # Django only
      - '**.js'
      - '**.jsx'
      - '**.ts'
      - '**.tsx'
```

### Change Target Branches

Edit the `on` section:

```yaml
on:
  pull_request:
    branches:
      - main
      - develop  # Changed from 'dev'
      - feature/*
```

---

## Cost Estimation

### Claude API Pricing (as of April 2026)

**Claude 3.5 Sonnet:**
- Input: $3 / 1M tokens
- Output: $15 / 1M tokens

### Per-Review Cost

**Typical PR Review:**
- Input tokens: ~4,000 (guidelines + diff)
- Output tokens: ~500 (review response)
- Cost: ~$0.03-0.05 per review

**Monthly Example:**
- 10 repos × 5 PRs/week = ~20 PRs/month
- 20 × $0.04 = **$0.80/month**
- Well under Claude Pro budget

### Claude Pro Membership

- **Cost**: $20/month
- **Includes**: 5M tokens/month = ~100,000 code reviews
- **Cost per review with Pro**: ~$0.0002 (essentially free)

---

## Best Practices

### 1. Keep PRs Focused

Small, focused PRs get better reviews:
- ✅ 50-500 line changes = excellent
- ⚠️ 500-2000 lines = good
- ❌ 2000+ lines = review may be incomplete

### 2. Include Context in PR Description

```markdown
## What This PR Does
Fixed N+1 query in user list endpoint

## Why
User list was loading user profiles for each user without select_related

## Testing
- [x] Tested locally with 1000 users
- [x] Performance improved from 5s to 100ms
```

Claude can see this in the diff and provide better context.

### 3. Act on Critical Issues

Issues marked **[CRITICAL]** or **[HIGH]** should be addressed before merging.

### 4. Review the Review

The automated review is a tool, not a replacement for human review. Always:
- ✅ Read the automated review
- ✅ Consider the suggestions
- ✅ Discuss with team if disagreeing
- ✅ Have a human review before merging

---

## Replicating to New Repositories

### Quick Copy (3 steps):

1. **Get the workflow file:**
   - Copy `code-review-nextjs.yml` (for Next.js)
   - Copy `code-review-django.yml` (for Django)

2. **Create directory:**
   ```bash
   mkdir -p .github/workflows
   ```

3. **Add file:**
   - Save as `.github/workflows/code-review-[tech].yml`
   - Commit and push
   - Done! API key is already in organization secrets

### Using GitHub Web UI:

1. Go to your new repo
2. Click **Add file** → **Create new file**
3. Type: `.github/workflows/code-review.yml`
4. Paste the workflow content
5. Commit

---

## FAQ

**Q: Will this cost a lot?**
A: No. With Claude Pro ($20/month), you get enough tokens for ~100,000 reviews. Even with free tier, typical usage is under $5/month.

**Q: Can I customize the guidelines per-project?**
A: Yes. Edit the `core_guidelines` and `{tech}_guidelines` variables in the workflow.

**Q: Does this block PRs from being merged?**
A: No. Reviews are informational only. You can still merge even with critical issues (though we recommend addressing them).

**Q: Can I see review history?**
A: Yes. All reviews appear as comments on PRs, so you can search through PR history.

**Q: What if I don't want a review for this PR?**
A: Add `[skip-review]` to your PR title to skip the workflow.

**Q: How do I update guidelines for all repos?**
A: Update the `core_guidelines` variable in one workflow, then manually update all repo workflows. Or use a script to automate this.

---

## Need Help?

1. Check GitHub Actions logs: **Actions** tab → click the failed workflow
2. Review Claude API status: https://status.anthropic.com
3. Check API key validity: https://console.anthropic.com


✅ IDEAL      ██░░░░░░░░   +200 -80    Full review, no truncation
✅ OK         ████░░░░░░   +400 -150   Covered comfortably  
⚠️ LARGE      ███████░░░   +700 -250   Borderline, review may miss things
❌ TOO BIG    ██████████   +1000 -300  Split this PR

