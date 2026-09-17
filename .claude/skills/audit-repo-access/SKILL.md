---
name: audit-repo-access
description: Prove aegis clone works and AEGIS_PR_TOKEN is scoped PR-only (not a broad credential)
user-invocable: true
---

# Audit Repo Access

1. Read `.env` for `AEGIS_REPO_URL`, `AEGIS_COMPONENT=audit`, optional `AEGIS_PR_TOKEN`.
2. `git ls-remote "$AEGIS_REPO_URL" HEAD` — record tip SHA.
3. Confirm `audit/` exists on tip.
4. If `AEGIS_PR_TOKEN` set:
   - `gh api user` with that token — note login
   - `gh api repos/hamidmatiny/aegis` must succeed
   - Attempt `gh api repos/hamidmatiny/aegis-ceo` (or another non-aegis repo) — **must fail** (403/404). If it succeeds, **FAIL** the audit: token is too broad; refuse to use it; escalate to aegis-infra/Hamid.
   - Confirm token is fine-grained / not a classic `ghp_` with org-wide repo scope if detectable.
5. Report: access ok|failed; tip SHA; PR token present|absent|too_broad.

Never claim PR capability without step 4 passing.
