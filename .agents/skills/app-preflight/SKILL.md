---
name: app-preflight
description: Run sanity checks before opening a PR. Reads "Sanity checks before PR" from AGENTS.md and runs the common baseline.
---

# App preflight

Read the **"Sanity checks before PR"** section in `<slug>/AGENTS.md` — that is
the authoritative list. The baseline every app in this repo shares:

## Checks (run from inside the `<slug>/` directory)

- **Python** (if `wyoming_*/` exists):
  ```
  python3 -c "import ast,glob;[ast.parse(open(f).read()) for f in glob.glob('wyoming_*/*.py')]"
  ```
- **YAML:** `yamllint config.yaml translations/*.yaml`
- **Shell / s6:** `shellcheck -x rootfs/etc/s6-overlay/s6-rc.d/*/run` (and `finish`)
- Only when asked (slow): `docker build .`
- Only when running locally: Wyoming smoke test (port in AGENTS.md)

## Output format
- One line per check: `OK` / `FAIL` (+ first error) / `SKIPPED` (+ why).
- Overall verdict: ready / not ready for PR.
