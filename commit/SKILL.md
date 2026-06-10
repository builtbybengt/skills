---
name: commit
description: Stage, commit, and push current changes using Conventional Commits 1.0.0. Use when asked to commit, push, ship changes, or create a git commit.
---

Stage changed files, write a Conventional Commits 1.0.0 message derived from the diff, commit, and push to `origin/<current-branch>`.

## Process

### 1. Gather context

Run in parallel:

```bash
git status
git diff HEAD
git log --oneline -5
```

Read `git diff HEAD` carefully — the commit message must reflect the actual changes.

### 2. Choose type and scope

| Type | When |
|---|---|
| `feat` | New user-visible feature |
| `fix` | Bug fix |
| `refactor` | Restructure without behavior change |
| `chore` | Tooling, deps, config, CI |
| `style` | Formatting, naming, no logic change |
| `perf` | Performance improvement |
| `docs` | Documentation only |
| `test` | Tests only |
| `build` | Build system or external deps |

**Scope** (optional): the area most affected — lowercase, no slashes. Examples: `proxy`, `billing`, `middleware`, `waitlist`, `onboarding`.

Breaking changes: append `!` after type/scope and add a `BREAKING CHANGE: <description>` footer.

### 3. Write the message

```
<type>[optional scope]: <description>

[optional body]

[optional footers]
```

- Description: imperative mood, lowercase, no period, ≤72 chars
- Body: explain *why*, not *what*
- `BREAKING CHANGE` must be uppercase

### 4. Stage and commit

Inspect `git status` for untracked files — never stage `.env*` or credential files. Stage specific files:

```bash
git add <files>
git commit -m "$(cat <<'EOF'
<type>[scope]: <description>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

### 5. Push

```bash
git push origin HEAD
```

If no upstream yet: `git push -u origin <branch>`. Never force-push to `main`.

## Examples from this repo

```
feat(proxy): extract tenant subdomain from mylaunch.site
fix(billing): use sessionClaims for bypass gating check
chore(middleware): re-export proxy as next.js entry point
refactor(onboarding): replace hardcoded domain with env var
```

## Gotchas

- Pre-commit hooks may reject the commit — fix the underlying issue, never use `--no-verify`
- Prefer listing files explicitly over `git add -A` to avoid staging secrets or unrelated files
- If the branch is already pushed, create a new commit instead of amending
