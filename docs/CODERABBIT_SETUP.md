## CodeRabbit — AI Code Review for GitHub

This guide documents how to add CodeRabbit AI code reviews to MAMS repositories, so every PR gets automated review feedback and quality gates. It covers both CLI setup and GitHub App integration.

Official references (verify latest details):
- CodeRabbit site: `https://www.coderabbit.ai/`
- CodeRabbit documentation: `https://docs.coderabbit.ai/`
- CodeRabbit CLI docs: `https://docs.coderabbit.ai/cli/`

---

## Part A: CodeRabbit CLI Setup (Local Development)

### CLI Prerequisites
- Git installed and configured
- Terminal/command prompt available
- Active internet connection for authentication
- **For Windows ARM:** Windows Subsystem for Linux (WSL) is required

### CLI Installation Steps

#### Option 1: Windows ARM with WSL (Recommended)

**Step 1: Install WSL (if not already installed)**

Open PowerShell as Administrator and run:

```powershell
wsl --install
```

This will install WSL with Ubuntu by default. Restart your computer when prompted.

**Step 2: Launch WSL**

After restart, launch Ubuntu from the Start menu or run:

```powershell
wsl
```

**Step 3: Install CodeRabbit CLI in WSL**

Once inside WSL (Ubuntu), run:

```bash
curl -fsSL https://cli.coderabbit.ai/install.sh | sh
```

**Note:** The correct URL is `cli.coderabbit.ai/install.sh` (not `install.coderabbit.ai`).

**Step 4: Reload Shell Configuration**

After installation, reload your shell:

```bash
# For Bash (default)
source ~/.bashrc

# Or if using Zsh
source ~/.zshrc
```

**Step 5: Verify Installation**

Check that CodeRabbit CLI is installed:

```bash
coderabbit --version
```

You should see the version number displayed.

#### Option 2: Native Windows (If Available)

If CodeRabbit releases a native Windows installer in the future, follow the official installation instructions from their documentation.

### CLI Authentication

**Step 1: Login to CodeRabbit**

Run the authentication command:

```bash
coderabbit auth login
```

This will:
- Open a browser window for authentication
- Prompt you to sign in or create a CodeRabbit account
- Complete the OAuth flow

**Step 2: Verify Authentication**

Check your authentication status:

```bash
coderabbit auth status
```

### Using CodeRabbit CLI

**Review Uncommitted Changes**

Navigate to your project directory and review uncommitted changes:

```bash
# Navigate to project root (in WSL)
cd /mnt/c/Users/ddpat/Desktop/ALL/Daytona_hackSprint

# Review uncommitted changes
coderabbit review --type uncommitted
```

**Review Specific Files**

Review specific files or directories:

```bash
# Review a specific file
coderabbit review backend/app/main.py

# Review a directory
coderabbit review backend/app/
```

**Review Git Diff**

Review changes between branches or commits:

```bash
# Review changes in current branch vs main
coderabbit review --type diff --base main

# Review specific commit
coderabbit review --type commit <commit-hash>
```

**Review Pull Request**

Review a GitHub PR (requires GitHub token):

```bash
coderabbit review --type pr --pr-number <number> --repo <owner/repo>
```

### CLI Configuration

Create a `.coderabbit.yml` file in your project root to configure review behavior:

```yaml
# .coderabbit.yml
language:
  - python
  - typescript
  - javascript

paths:
  include:
    - backend/
    - frontend/
  exclude:
    - node_modules/
    - __pycache__/
    - *.pyc
    - .git/

review:
  depth: contextual
  large_file_threshold: 1000
  large_diff_threshold: 500
```

### CLI Troubleshooting

**Issue: Command not found after installation**

**Solution:**
```bash
# Check if the binary is in PATH
echo $PATH | grep coderabbit

# Manually add to PATH if needed (add to ~/.bashrc)
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
```

**Issue: Authentication fails**

**Solution:**
1. Clear authentication cache: `coderabbit auth logout`
2. Try login again: `coderabbit auth login`
3. Ensure browser can open (check firewall settings)

**Issue: WSL path issues with Windows files**

**Solution:**
- Access Windows files via `/mnt/c/` in WSL
- Example: `cd /mnt/c/Users/ddpat/Desktop/ALL/Daytona_hackSprint`
- Consider cloning the repo inside WSL for better performance

---

## Part B: GitHub App Integration (PR Reviews)

### 1) What We Want
- Automated PR reviews with clear, actionable comments
- Consistent quality bar across backend (Python) and frontend (Next.js/TypeScript)
- Non‑blocking in dev branches, optional blocking in `main` via branch protection
- Minimal configuration kept in‑repo for transparency

### 2) Prerequisites
- GitHub admin/owner permissions for installation
- Target repositories selected (single repo or organization‑wide)
- Agreement on default policies (blocking vs advisory, review scope)

### 3) Installation (to perform via official Docs)
1. From `https://www.coderabbit.ai/` open Docs and follow the GitHub App install flow.
2. Choose the organization/account and select which repositories to enable.
3. Grant required permissions (read code, read/write PRs & comments, checks). Review scopes in the installer.

### 4) Configuration Strategy (kept in repo)
Maintain a repo‑level configuration file (see keys and schema in `https://docs.coderabbit.ai/`) to control behavior per project. Recommended dimensions to document/configure:
- Languages and paths to include/exclude (e.g., ignore generated code, lockfiles)
- Review depth (diff‑only vs. contextual), large‑file/large‑diff thresholds
- Severity levels for findings and whether to open PR comments vs a single summary
- Optional labels or keywords to skip/limit review (e.g., `chore:` commits)
- Monorepo directory‑specific overrides (backend vs frontend)

Notes:
- Do not hardcode secrets. Keep config human‑readable and versioned.
- Keep defaults conservative; widen scope incrementally after first week.


### 5) Branch Protection & Quality Gates
Use GitHub Branch Protection Rules on `main` (and other critical branches) to require:
- CodeRabbit check status is passing (advisory in early rollout; enforce later)
- Status checks from tests/lint/typecheck still apply as usual

Rollout sequence:
1. Week 1: advisory only; collect feedback
2. Week 2: require passing on `main` with “request changes” severity limited to high‑confidence issues
3. Iterate thresholds based on noise vs. value


### 6) Developer Workflow
- Open PR → CodeRabbit posts review comments and/or summary
- Author addresses feedback; push updates → CodeRabbit re‑runs on delta
- Maintainers ensure high‑severity findings are addressed or annotated with rationale
- Merge once all required checks pass


### 7) Privacy & Security
- Code shared via GitHub App context; review permissions during install
- Avoid including secrets in code/PRs; enforce pre‑commit hooks to block
- Use repo ignores to exclude sensitive or vendored content


### 8) Testing the Integration
Create a small test PR in both repos (backend and frontend):
- Introduce a minor smell (unused import, overly broad exception) → verify comment
- Add a safe refactor and ensure low noise
- Confirm re‑review triggers on new commits


### 9) Troubleshooting
- No comments appearing:
  - Confirm the GitHub App is installed for the repo and has permission to write comments
  - Ensure the repo is enabled in the CodeRabbit dashboard
  - Check PR event types (opened/synchronize) are firing
- Excessive/noisy feedback:
  - Tighten include paths, raise severity threshold, or switch to summary‑only mode for certain paths
- Missed high‑priority issues:
  - Review language settings; ensure the relevant file types are included


### 10) Cost & Usage Considerations
- Scope reviews to changed files; avoid scanning vendored/generated code
- Calibrate thresholds to reduce re‑run load from large PRs
- Prefer smaller, frequent PRs for better signal‑to‑noise


### 11) Rollout Checklist (Docs‑Only)
- [ ] Install GitHub App for selected repos via Docs
- [ ] Add repo‑level configuration file (schema from official docs)
- [ ] Set advisory mode for `main` in week 1
- [ ] Enable required check for `main` in week 2 with tuned thresholds
- [ ] Link this doc from README and the integration index


