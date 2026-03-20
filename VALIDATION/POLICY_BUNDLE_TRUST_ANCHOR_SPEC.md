# POLICY_BUNDLE_TRUST_ANCHOR_SPEC

## Status
Normative (Binding)

## Purpose
Define the signer trust anchor model for policy bundle validation so that signer authority is deterministic, non-mutable at runtime, and fail-closed.

## Core Principles
1. Determinism: identical run inputs MUST produce identical validation outcomes.
2. Boundary: implementations MUST validate authority artifacts and MUST NOT create new authority.
3. Immutable Artifact Principle: runtime authority MUST come only from pinned, sealed, content-addressed artifacts.
4. No Runtime Trust Mutation: trust sources MUST NOT be writable or replaceable during execution.

## Trust Anchor Model
1. Root of trust is the embedded root signer set shipped with implementation release artifacts.
2. Embedded root signer set MUST be immutable for a given implementation release.
3. Runtime MUST NOT fetch trust anchors from network, external PKI, environment variables, or mutable local state.
4. Embedded root signer set MUST define only root signer identities and root signer public authority metadata.
5. All delegated signer authority MUST chain to an embedded root signer.

## Signer Registry Model
1. `SIGNERS.json` is allowed only as a sealed signer registry artifact.
2. `SIGNERS.json` MUST be versioned.
3. `SIGNERS.json` MUST be digest-pinned by SHA-256.
4. `SIGNERS.json` MUST be signed by an embedded root signer identity.
5. Unsigned or digest-mismatched `SIGNERS.json` MUST be rejected.
6. Runtime MUST NOT use a repository working copy as trust source.
7. Runtime signer registry source MUST be the pinned run input copy only.

## Admission Rules
1. A signer becomes valid only through signer-registry admission.
2. Signer-registry admission MUST be human-approved and sealed before runtime use.
3. Delegated signer admission MUST include:
   - `signer_id`
   - `parent`
   - `active`
   - explicit scope (`allowed_bundles`, `allowed_plans`, `countries`)
4. Admission artifacts MUST be immutable once sealed.
5. Runtime MUST NOT admit new signers.

## Revocation Rules
1. Revocation is performed by creating a new sealed signer-registry version.
2. Revocation MUST be represented as explicit signer state (`active=false`) in the new admitted version.
3. Past runs MUST remain reproducible by validating against the signer-registry version pinned in that run.
4. Runtime MUST NOT retroactively reinterpret historical runs using newer signer registries.

## Execution Binding
1. Run initialization MUST materialize the signer registry into a sealed run-local input artifact.
2. Run initialization MUST record signer-registry SHA-256 in a run-local trust-anchor digest artifact.
3. Policy bundle validation MUST use only the run-local pinned signer registry.
4. Policy bundle validation MUST verify:
   - signer registry digest matches pinned digest
   - signer registry signature chains to embedded root signer set
   - bundle signer chains to validated signer registry
5. Policy bundle validation MUST reject any trust source not bound to pinned run-local artifacts.

## Chain of Trust
1. Delegation is allowed only through explicit parent-child linkage.
2. Chain traversal MUST terminate at an embedded root signer.
3. Each chain node MUST exist and be active.
4. Cycles MUST be rejected.
5. Effective scope MUST be derived by strict intersection across delegated chain scopes.
6. Scope widening at any child level MUST be rejected.

## Failure Semantics
Validation MUST FAIL-CLOSED when any of the following occurs:
1. signer registry artifact missing
2. signer registry digest mismatch
3. signer registry signature invalid
4. signer not found
5. signer inactive
6. invalid or cyclic signer chain
7. scope missing for delegated signer
8. scope widening detected
9. effective scope empty
10. bundle outside effective scope

No fallback source, partial acceptance, or warning-only mode is allowed.

## Final Statement
Policy bundle signer authority is valid only when bound to the embedded root trust anchor, a sealed and digest-pinned signer registry, and a fail-closed delegated chain validation process executed against immutable run inputs.
