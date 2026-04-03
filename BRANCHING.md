# Git Flow Branching Strategy

This project uses **Git Flow** for version control and branch management.

## Branch Structure

### Long-lived Branches (Protected)
- **`main`** - Production-ready code only
  - Always stable and deployable
  - Each commit represents a release
  - Tag releases with version numbers
  
- **`dev`** - Development branch (integration branch)
  - Contains latest development changes
  - Used as base for feature branches
  - Should be relatively stable

### Short-lived Branches (Feature/Hotfix)
- **`feature/*`** - Feature development
  - Branch from: `dev`
  - Example: `feature/user-authentication`, `feature/api-integration`
  - Merge back to: `dev` via pull request
  - Delete after merge

- **`hotfix/*`** - Critical production fixes
  - Branch from: `main`
  - Example: `hotfix/security-patch`, `hotfix/critical-bug`
  - Merge back to: `main` AND `dev`
  - Delete after merge

## Workflow

### 1. Starting a New Feature
```bash
# Update dev branch
git checkout dev
git pull origin dev

# Create feature branch
git checkout -b feature/your-feature-name
```

### 2. Working on Your Feature
```bash
# Commit regularly with clear messages
git add .
git commit -m "Add specific functionality"
git push origin feature/your-feature-name
```

### 3. Finishing a Feature
```bash
# Ensure dev is latest
git checkout dev
git pull origin dev

# Merge feature into dev
git merge --no-ff feature/your-feature-name

# Delete feature branch
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name

# Push to dev
git push origin dev
```

### 4. Creating a Release (main → production)
```bash
# Create release commit on main
git checkout main
git pull origin main
git merge --no-ff dev

# Tag the release
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main
git push origin v1.0.0

# Merge back to dev
git checkout dev
git merge --no-ff main
git push origin dev
```

### 5. Hotfixes (Emergency fixes)
```bash
# Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/your-fix-name

# Fix the issue and commit
git add .
git commit -m "Fix critical issue"

# Merge to main
git checkout main
git merge --no-ff hotfix/your-fix-name
git tag -a v1.0.1 -m "Hotfix release"
git push origin main
git push origin v1.0.1

# Merge to dev
git checkout dev
git merge --no-ff hotfix/your-fix-name
git push origin dev

# Delete hotfix branch
git branch -d hotfix/your-fix-name
git push origin --delete hotfix/your-fix-name
```

## Commit Message Guidelines

Use clear, descriptive commit messages:
- `feat: Add user login feature`
- `fix: Correct database connection issue`
- `docs: Update README with setup instructions`
- `style: Format code according to standards`
- `refactor: Simplify authentication logic`
- `test: Add unit tests for payment module`

## Best Practices

✅ **DO:**
- Keep commits small and focused
- Use `--no-ff` when merging to preserve branch history
- Delete feature branches after merging
- Pull latest changes before starting new work
- Tag releases on `main` branch
- Keep `main` and `dev` in sync

❌ **DON'T:**
- Push directly to `main` or `dev`
- Use force push without coordination
- Leave abandoned feature branches
- Mix multiple unrelated features in one branch
- Commit directly to production-ready code

## Current Status

Your repository currently has:
- ✅ `main` - Production branch
- ✅ `dev` - Development branch
- 🔹 `frommain` - Merge branch (may be old)

**Next Steps:**
1. Clean up old branches if no longer needed
2. Start using `feature/*` for new work
3. Keep `main` stable for production use
