---
name: flag-finding
description: Escalate a real audit finding to aegis-ceo with confirmed delivery
user-invocable: true
---

# Flag Finding

1. Package: component, file paths, evidence, recommended PR (if any).
2. `mcp__trinity__chat_with_agent` → `aegis-ceo`. Claim escalated **only** after confirmed delivery.
3. On failure: operator-queue alert + honest Slack close-out.
4. Append to `memory/findings.md` with delivery status.
