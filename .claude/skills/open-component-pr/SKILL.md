---
name: open-component-pr
description: Open a PR against hamidmatiny/aegis for audit only — never merge
user-invocable: true
---

# Open Component PR

**Hard stops:** no merge, no force-push to main, no prod credentials, no edits outside `audit/` (and explicitly co-owned shared policy files).

1. `/audit-repo-access` must show scoped `AEGIS_PR_TOKEN` (not too_broad). If absent/too_broad — stop.
2. Branch from latest main: `component/audit/<short-slug>`.
3. Commit only in-scope files. Push with `AEGIS_PR_TOKEN`.
4. `gh pr create` against `hamidmatiny/aegis` — request review; do **not** merge; do **not** approve your own PR.
5. Record PR URL in `memory/findings.md`.

If branch protection rejects the push to main, that is correct — you should never push to main.
