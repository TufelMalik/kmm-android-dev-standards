# Rule 4: Git Branch Management & Workflow

> **Official References:**  
> - [GitFlow Model](https://nvie.com/posts/a-successful-git-branching-model/) (Vincent Driessen)
> - [Conventional Commits](https://www.conventionalcommits.org/)
> - [Semantic Versioning](https://semver.org/)
> - [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)

---

## Rule: Follow GitFlow or GitHub Flow for Branch Management

**Why:** GitFlow is the industry-standard branching model that provides a robust framework for managing releases, features, and hotfixes. It prevents conflicts, enables parallel development, and creates a clear audit trail.

**When to use:**
- **GitFlow** - Projects with scheduled releases (mobile apps, desktop software)
- **GitHub Flow** - Continuous deployment (web services, APIs)

---

## Branch Types (GitFlow Specification)

| Branch | Purpose | Base Branch | Merge To | Naming |
|--------|---------|-------------|----------|--------|
| `main` | Production-ready code | - | - | `main` |
| `develop` | Integration branch | `main` | `main` (via release) | `develop` |
| `feature/` | New features | `develop` | `develop` | `feature/<issue-id>-<description>` |
| `bugfix/` | Bug fixes | `develop` | `develop` | `bugfix/<issue-id>-<description>` |
| `release/` | Release preparation | `develop` | `main` + `develop` | `release/<version>` |
| `hotfix/` | Urgent production fixes | `main` | `main` + `develop` | `hotfix/<version>-<description>` |

---

## Branch Naming Examples

```bash
# Features
feature/123-user-authentication
feature/456-payment-integration
feature/789-push-notifications

# Bug fixes
bugfix/234-login-crash
bugfix/567-memory-leak

# Releases
release/1.2.0
release/2.0.0-beta

# Hotfixes
hotfix/1.1.1-payment-crash
hotfix/1.2.1-security-patch
```

---

## GitFlow Workflow

### 1. Feature Development

```bash
# Start new feature from develop
git checkout develop
git pull origin develop
git checkout -b feature/123-user-profile

# Work on feature (commit regularly)
git add .
git commit -m "feat(auth): add user profile screen"
git commit -m "feat(auth): implement profile update API"
git commit -m "test(auth): add profile screen tests"

# Keep branch up to date
git checkout develop
git pull origin develop
git checkout feature/123-user-profile
git merge develop  # or: git rebase develop

# Push feature branch
git push origin feature/123-user-profile

# Create Pull Request → develop
# After approval, merge via PR
```

### 2. Release Preparation

```bash
# Create release branch
git checkout develop
git pull origin develop
git checkout -b release/1.2.0

# Bump version, update changelog, final testing
git commit -m "chore(release): bump version to 1.2.0"
git commit -m "fix(ui): correct button alignment for release"

# Merge to main
git checkout main
git merge --no-ff release/1.2.0
git tag -a v1.2.0 -m "Release version 1.2.0"
git push origin main --tags

# Merge back to develop
git checkout develop
git merge --no-ff release/1.2.0
git push origin develop

# Delete release branch
git branch -d release/1.2.0
git push origin --delete release/1.2.0
```

### 3. Hotfix (Urgent Production Fix)

```bash
# Create hotfix from main
git checkout main
git checkout -b hotfix/1.2.1-payment-crash

# Fix the issue
git commit -m "fix(payment): prevent crash on null transaction"

# Merge to main
git checkout main
git merge --no-ff hotfix/1.2.1-payment-crash
git tag -a v1.2.1 -m "Hotfix version 1.2.1"
git push origin main --tags

# Merge to develop
git checkout develop
git merge --no-ff hotfix/1.2.1-payment-crash
git push origin develop

# Delete hotfix branch
git branch -d hotfix/1.2.1-payment-crash
git push origin --delete hotfix/1.2.1-payment-crash
```

---

## Conventional Commits (Official Specification)

**Format:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Commit Types

| Type | Description |
|------|-------------|
| `feat` | New feature for the user |
| `fix` | Bug fix for the user |
| `docs` | Documentation only changes |
| `style` | Code style (formatting, semicolons) |
| `refactor` | Code change (no bug fix or feature) |
| `perf` | Performance improvement |
| `test` | Adding or updating tests |
| `build` | Build system or dependencies |
| `ci` | CI configuration files |
| `chore` | Other changes (not src/test) |

### Examples

```bash
# Features
git commit -m "feat(auth): add Google Sign-In integration"
git commit -m "feat(ui): implement dark mode theme"
git commit -m "feat(api): add user profile endpoint"

# Fixes
git commit -m "fix(payment): prevent crash on null transaction"
git commit -m "fix(ui): correct button alignment on login"
git commit -m "fix(database): resolve migration conflict"

# Breaking change
git commit -m "feat(api): change authentication response structure

BREAKING CHANGE: API now returns token in 'auth_token' field instead of 'token'"

# Other types
git commit -m "docs(readme): update installation instructions"
git commit -m "refactor(auth): extract validation logic to helper"
git commit -m "test(login): add unit tests for ViewModel"
git commit -m "chore(deps): update Compose to 1.5.4"
git commit -m "ci(github): add automated release workflow"
git commit -m "perf(list): implement lazy loading for tasks"
```

---

## Branch Protection Rules

```yaml
# Apply to main and develop branches

Required:
  - Require pull request reviews (min 1 approval)
  - Require status checks to pass (CI/CD)
  - Require branches to be up to date before merging
  - Automatically delete head branches after merge

Optional:
  - Require signed commits
  - Require deployments to succeed
  - Lock branch (prevent direct pushes)
```

---

## Semantic Versioning

**Format:** `MAJOR.MINOR.PATCH` (e.g., `1.4.2`)

| Version | When to Increment |
|---------|-------------------|
| **MAJOR** (1.x.x) | Breaking changes, incompatible API changes |
| **MINOR** (x.4.x) | New features, backwards-compatible |
| **PATCH** (x.x.2) | Bug fixes, backwards-compatible |

```bash
git tag -a v1.0.0 -m "Initial release"
git tag -a v1.1.0 -m "Add user profiles feature"
git tag -a v1.1.1 -m "Fix login crash"
git tag -a v2.0.0 -m "Breaking change: new API structure"

git push origin --tags
```

---

## Alternative: GitHub Flow (Simpler)

For continuous deployment:

```
main (always deployable)
  ↑
  └── feature/user-auth → PR → main
  └── bugfix/login-issue → PR → main
```

```bash
# 1. Create branch from main
git checkout main
git pull origin main
git checkout -b feature/new-feature

# 2. Commit work
git commit -m "feat: add new feature"

# 3. Push and create PR to main
git push origin feature/new-feature

# 4. After review, merge to main (auto-deploys)

# 5. Delete branch
git branch -d feature/new-feature
```

---

## Best Practices Summary

✅ Use GitFlow for scheduled releases (mobile/desktop apps)  
✅ Use GitHub Flow for continuous deployment (web apps)  
✅ Follow Conventional Commits for commit messages  
✅ Use Semantic Versioning for release tags  
✅ Protect `main` and `develop` with PR requirements  
✅ Require at least 1 code review approval  
✅ Ensure CI/CD passes before merging  
✅ Delete branches after merging  
✅ Tag all releases with version numbers  

---

*Following these practices ensures clean git history and professional project management.*
