---
name: audit-component
description: Real audit of the audit component — coverage, known gaps, stale docs
user-invocable: true
---

# Audit Component (`audit`)

1. Ensure `/audit-repo-access` passed (or run it).
2. Clone/fetch into `~/repos/aegis` (read). Focus on `audit/` only.
3. Inventory: Go/Python file count, `*_test.go` / test count, README Known limitations.
4. Run tests if practical (`docker run ... go test ./...` in component dir) — record pass/fail honestly.
5. Diff README claims vs code (stale "future work", missing emitters, etc.).
6. Append findings to `memory/audit-log.md` and update `memory/backlog.md`.
7. If `mcp__trinity__report` available: `report_type: aegis_audit.component_audit`.

Never fabricate coverage numbers.
