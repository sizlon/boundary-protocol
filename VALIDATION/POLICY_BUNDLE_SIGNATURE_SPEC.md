# Policy Bundle Signature Spec

## Purpose
Every policy bundle must be signed and bound to an authorized signer identity before it can be used by runtime.

## Signature Requirement
A policy bundle manifest must include:

- `signer`: non-empty string signer id
- `signature`: object with required fields:
  - `algorithm`: must be `sha256`
  - `digest`: 64-char lowercase hex digest

The manifest schema enforcing this is implementation-defined, but it MUST require the fields above.

## Signature Binding
- A valid signature binds the bundle to the declared signer identity.
- This binding is authoritative and MUST NOT be inferred or overridden.
- After signature verification, signer authority MUST be validated through the signer chain (see [POLICY_BUNDLE_SIGNER_CHAIN_SPEC.md](POLICY_BUNDLE_SIGNER_CHAIN_SPEC.md)).
- A bundle is valid only if all of the following hold:
  - signature is valid
  - signer is valid
  - signer has authority over the bundle

## Signer Binding
Signer identity is resolved from the sealed signer registry artifact admitted by the implementation.

Rules:
- signer must exist
- signer must be active
- delegated signer must have valid active parent chain
- delegated signer must pass scope checks (bundle, plan, country)

## Validation Steps
Runtime validator executes deterministic checks in this order:

1. Load bundle manifest.
2. Validate manifest against `policy_bundle.schema.json`.
3. Validate `signer` field exists and is non-empty.
4. Validate signer registry entry exists and is active.
5. Validate delegated authority chain (parent exists and active).
6. Validate signature digest:
   - remove top-level `signature` object
   - canonicalize JSON (UTF-8, sorted keys, deterministic separators)
   - compute SHA256 hex
   - compare with `signature.digest`
7. Derive effective authority via signer chain:
   - compute effective scope (countries, allowed_plans, allowed_bundles)
   - ensure no scope widening
8. Validate bundle against effective scope:
   - bundle_id matches allowed patterns
   - bundle plans ⊆ allowed_plans
   - bundle country ∈ countries

## Failure Rules
Validation must fail closed on any of the following:

- missing signature object
- missing signer field
- unknown signer
- inactive signer
- invalid delegated chain
- signature algorithm mismatch
- signature digest mismatch
- signer has no authority over bundle
- effective scope is empty
- scope violation

No fallback, no bypass mode, no runtime inference.

## Deterministic Guarantees
- No external key lookup.
- No network dependency.
- No non-deterministic inputs.
- Same manifest + same signer registry => same validation outcome.

Status: READY
