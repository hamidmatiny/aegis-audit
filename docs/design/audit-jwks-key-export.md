# Design: Offline Trust for AEGIS Audit Receipts (JWKS & Key Export)

## Problem Statement

AEGIS Audit produces tamper-evident `AuditReceipt`s signed with Ed25519. While receipt integrity is fully self-contained and verifiable offline if the verifier has the correct public key, AEGIS currently lacks a durable, independently-fetchable trust anchor for historical signing keys.

Currently, if an `AuditReceipt` leaves the native AEGIS deployment, trust terminates at the originating deployment. We lack:
1. `GET /v1/keys/{id}`: No endpoint to fetch the public key for a specific `signer_key_id`.
2. JWKS/Key History: No standardized, published format (like JWKS) for historical key verification.

As noted in `audit/README.md` and issue #63, this is a known gap preventing offline third-party verification of historical receipts.

## Goals

1.  **Durable Trust Anchors**: Enable offline verification of historical `AuditReceipt`s by providing a mechanism to fetch public keys for past `signer_key_id`s.
2.  **Standardized Key Publication**: Implement a JWKS-compatible endpoint or key-history publication that aligns with industry standards for signing key discovery.
3.  **Independence**: Ensure verification does not require a live callback to the originating AEGIS deployment if the key history is cached or fetched beforehand.

## Non-Goals

1.  Re-signing or wrapping existing `AuditReceipt`s.
2.  Full decision-to-execution correlation (this is acknowledged as a separate, future gap).
3.  Implementing a blockchain or transparency log for keys (standardized API publication is sufficient for the current threat model).

## Proposed API

### `GET /v1/keys` (or `.well-known/jwks.json`)

Expose a standardized JWKS endpoint that lists all active and historical public keys known to the deployment.

**Response Structure (JWKS):**

```json
{
  "keys": [
    {
      "kty": "OKP",
      "crv": "Ed25519",
      "kid": "prod-key-2026-09",
      "x": "...", 
      "use": "sig"
    },
    {
      "kty": "OKP",
      "crv": "Ed25519",
      "kid": "prod-key-2026-01",
      "x": "...",
      "use": "sig"
    }
  ]
}
```

-   `kid`: Maps directly to `signer_key_id` in `AuditReceipt`.
-   `x`: The Ed25519 public key in base64url format.

### `GET /v1/keys/{id}`

For direct lookup of a single key by ID.

**Response:**
Returns the JWK object for the requested key ID.

## Historical Key Publication & Rotation

-   **Persistence**: The service will read from `AEGIS_AUDIT_SIGNING_KEYS_HISTORY` (the existing rotation mechanism) to populate the JWKS set.
-   **Stability**: The JWKS will be cached and served statically where possible to ensure high availability for offline verifiers.

## Export Packaging

-   **Single Receipt**: Remains the primary portable unit.
-   **Optional Signed Manifest**: Introduce an optional `AuditReceiptManifest` that binds a collection of receipts via a Merkle root or combined hash, signed by the current active key.

## Trust Model

Offline verifiers will fetch the JWKS once (or via out-of-band delivery) and trust the Ed25519 signatures based on the public keys validated by the `kid` mapping.

## Migration / Rollout

1.  Implement `GET /v1/keys` and `GET /v1/keys/{id}` in `audit` service.
2.  Populate endpoints using current `AEGIS_AUDIT_SIGNING_KEYS_HISTORY`.
3.  Update documentation in `audit/README.md`.
4.  Deprecate reliance on internal-only key history management for third parties.

## Open Questions

1.  Should `AuditReceipt` include a full JWKS URL in the header for easier discovery?
2.  What is the desired retention period for historical keys in the published JWKS?
3.  Is a transparency log required for key publication, or is a signed JWKS sufficient for the risk profile?
