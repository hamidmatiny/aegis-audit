# Audit log

## 2026-09-17T16:46Z — hire-day seed audit

- Inventory: 23 files, 19 Go, **8** tests (best coverage of Phase 1 set).
- README known gaps: gRPC AuditService; cross-service auto-emit still manual POST; public key export / JWKS not exposed.
- Residual risk (documented): receipts ≠ detector correctness — map which enforcer services actually emit today (agent-gate/policy best-effort).
- Key rotation history path exists (`AEGIS_AUDIT_SIGNING_KEYS_HISTORY`) — confirm tests still cover unknown key id fail-closed.
