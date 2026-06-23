# Stage355: Signature Key Status Verification & Revocation Enforcement Layer

Stage355 extends Stage354 by verifying signature key status and initializing fail-closed revocation enforcement logic.

Stage354 created the signature key rotation ledger.

Stage355 reads that ledger and checks whether key states can be trusted for verification decisions.

---

## What Stage355 Adds

Stage355 adds:

- Stage354 key ledger loading
- Key status verification
- Revocation safety checks
- Expired / revoked / superseded key safety checks
- Signing-time validity readiness
- PQC ML-DSA intent_only protection
- Stage354 entry_hash binding as Stage355 previous_hash
- Stage355 entry_hash generation
- Fail-closed rejection rules

---

## Main Purpose

Stage355 answers this question:

```text
Was the signing key valid for the claimed signing context?

In simple terms:

Stage354 creates the key registry.
Stage355 checks whether the key was allowed to be used.
Inputs

Stage355 reads:

docs/keys/stage354_key_rotation_ledger.json
docs/keys/stage354_key_rotation_result.json
Outputs

Stage355 generates:

docs/keys/stage355_key_status_verification.json
docs/keys/stage355_revocation_enforcement_result.json
docs/keys/stage355_key_status_summary.txt
Decision

Current decision:

accept_verification_ready

This means:

Stage354 key ledger exists
Stage354 ledger chain is valid
Stage354 entry_hash is bound as Stage355 previous_hash
Key status records were checked
No private keys were published
PQC ML-DSA remains intent_only
No revoked key active claim was detected
Fail-Closed Rules

Stage355 rejects if:

Stage354 ledger is missing
Stage354 result is missing
Stage354 ledger chain is invalid
key records are missing
private key publication is detected
revoked key is claimed as active
PQC ML-DSA intent_only is falsely claimed as active
Stage354 entry_hash cannot be bound as previous_hash
Safety Boundary

Stage355 does not:

publish private keys
publish raw secrets
perform real production key rotation
claim real Rekor inclusion
claim real PQC signature verification
claim fake active PQC keys
Relationship to Stage354

Stage354:

Records key lifecycle assumptions, threats, guarantees, and key status metadata.

Stage355:

Uses that ledger to verify key status and enforce fail-closed safety rules.
License

MIT License
