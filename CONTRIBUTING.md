# Contributing to OmegaFounders

This repo is designed for **high-velocity multi-contributor collaboration** — cohort members posting daily logs, builders improving the landing page, and the community growing the skill library.

## Ground Rules

1. **One PR per day per person** (for daily logs)
2. **One branch per feature/fix** (never commit directly to `main`)
3. **PRs should be small and reviewable** — aim for <200 lines changed
4. **CI must pass** before merging
5. **Be concrete, not vague** — in PR descriptions and commit messages

---

## Branch Naming

```
feat/your-feature-name      # new feature
fix/bug-description         # bug fix
docs/what-you-updated       # documentation
learn/github-handle-day-N   # daily log submission
refactor/what-changed       # code cleanup
```

---

## Commit Message Format

```
type(scope): short description under 72 chars

Optional body — explain WHY not just WHAT.
Co-Authored-By: Name <email>
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`

Examples:
```
feat(landing): add K-shaped market section with 7 skills grid
fix(skill): correct Haiku pricing from $0.80 to $1.00 per MTok
docs(day-04): add day 4 MCP integration learn-build-teach log
```

---

## Workflow for Cohort Members (Daily Log)

Each day you complete, submit a PR with your daily log:

```bash
# 1. Always start from latest main
git checkout main
git pull origin main

# 2. Create your day branch
git checkout -b learn/YOUR_HANDLE-day-N

# 3. Copy the template
cp learn-build-teach/TEMPLATE.md learn-build-teach/YOUR_HANDLE/day-N.md

# 4. Fill it out (see template for required sections)
# ... write your log ...

# 5. Stage and commit
git add learn-build-teach/YOUR_HANDLE/day-N.md
git commit -m "docs(day-N): YOUR_HANDLE day N learn-build-teach log"

# 6. Push and open PR
git push origin learn/YOUR_HANDLE-day-N
# Then open a PR on GitHub using the daily log PR template
```

**PR title format**: `[Day N] YOUR_HANDLE — what you shipped`

---

## Workflow for Feature Contributors

```bash
git checkout main && git pull origin main
git checkout -b feat/your-feature

# ... make your changes ...

git add <specific files>          # never: git add -A
git commit -m "feat(scope): description"
git push origin feat/your-feature
# Open PR using the feature PR template
```

---

## Review Process

- **Daily logs**: 1 approval needed (any cohort member or maintainer)
- **Landing page changes**: 1 approval from @wjlgatech
- **Skill changes**: 1 approval from @wjlgatech + CI must pass
- **CI workflow changes**: maintainer-only

---

## What NOT to Commit

- `*.pdf` files (use `docs/` with markdown links instead, or reference external)
- API keys, tokens, `.env` files (ever, under any circumstances)
- `node_modules/`, `.DS_Store`, build artifacts
- Binary files >1MB without discussion

---

## Questions?

Open an issue with the `question` label or post in the cohort Slack/Discord.
