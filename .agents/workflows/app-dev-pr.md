---
name: app-dev-pr
description: Branch from main, run preflight, open a PR, then publish a release to ship. Use at the start of any change in this repo.
---

# Integrate a change

Single-app repo — only `main` branch, no `dev`.

## 1. Branch off `main`
```
git fetch origin && git switch -c <type>/<short-desc> origin/main
```

## 2. Pre-merge checks
Run [[app-preflight]].

## 3. PR to `main`
Use a conventional-commit-style PR **title** — the autolabeler labels it automatically:

| Prefix | Label | Release section |
|---|---|---|
| `feat:` | enhancement | New Features |
| `fix:` / `perf:` / `revert:` | fix | Bug Fixes |
| `chore:` / `ci:` / `docs:` / `refactor:` | chore | Maintenance |

No scope needed (whole repo is one app). Squash merge is fine.
```
gh pr create --base main --title "feat: <subject>" --body "..."
```

## 4. Publish a release (when ready to ship)
1. release-drafter has already updated the `v<next-patch>` draft.
2. Review draft notes on GitHub → **Publish release**.
3. `build.yml` triggers → pushes multi-arch GHCR image.
4. `repository_dispatch` → ha-apps opens `chore(<slug>): release <ver>` sync PR.
