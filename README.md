# AEGIS Audit Owner

Track B SE I owner of `audit` in [hamidmatiny/aegis](https://github.com/hamidmatiny/aegis).

## Scope

Own tamper-evident Ed25519-signed decision receipts (Postgres store, verify, key rotation history).

May open branches/PRs. **No** merge to `main`, **no** production deploy credentials.

## Skills

| Skill | Purpose |
|-------|---------|
| `/audit-repo-access` | Prove clone + (optional) PR token scopes |
| `/audit-component` | Real audit of `audit` |
| `/open-component-pr` | Open a PR against aegis (never merge) |
| `/flag-finding` | Escalate to aegis-ceo with confirmed delivery |

## Setup

```bash
cp .env.example .env
# Mint fine-grained PAT scoped ONLY to hamidmatiny/aegis (Contents R/W, PRs R/W)
# then set AEGIS_PR_TOKEN in .env / Trinity credentials inject
```

Reports to `aegis-ceo`. Tier/model owned by `aegis-infra` (free-pool SE I).
