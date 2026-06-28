---
name: letsgo
description: "Ship a focused code slice. Validates gates, runs tests, checks dirty tree, and hands off to Closer for release. Use when ready to commit and push a scoped change."
---

# Letsgo

Ship the current scoped slice through the standard release path.

## Steps

1. Run `npm run agent:governance:validate` — verify agent authority
2. Run `git diff --check` — confirm clean tree
3. Run `npm run close:validate` — check release gates
4. Stage specific files (never `git add .` — risk of committing secrets or unrelated changes)
5. Commit with a scoped message following conventional commits
6. Push to the feature branch

Never skip gates. Never use `--no-verify`. If a gate fails, fix it — do not bypass.
