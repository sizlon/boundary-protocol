# POLICY_BUNDLE_TEMPORAL_VALIDITY_SPEC

## Status
Normative (Binding)

## Purpose
Define deterministic temporal validity rules for signer authority and policy bundle signatures, eliminating replay, stale signer usage, and clock-dependent ambiguity.

## Time Model
1. Validation time is `evaluation_epoch`.
2. `evaluation_epoch` MUST be provided in run inputs as an integer UNIX epoch in UTC seconds.
3. `evaluation_epoch` MUST be immutable for the run.
4. `evaluation_epoch` MUST be recorded in run artifacts and included in run sealing evidence.
5. Runtime wall-clock, OS time, NTP, and environment time MUST NOT be used.

## Signer Validity
Signer registry entries MUST include:
- `valid_from` (integer UNIX epoch seconds, UTC)
- `valid_until` (integer UNIX epoch seconds, UTC)

Rules:
1. Both fields are mandatory.
2. `valid_from < valid_until` MUST hold.
3. Validity interval is:
   - inclusive start: `valid_from <= t`
   - exclusive end: `t < valid_until`

## Signature Requirements
Policy bundle signature payload MUST include:
- `issued_at` (integer UNIX epoch seconds, UTC)

Rules:
1. `issued_at` is mandatory.
2. `issued_at` MUST be covered by signature (inside signed canonical payload).
3. `issued_at` MUST be canonicalized under canonical JSON rules.
4. Missing or non-integer `issued_at` is invalid.

## Validation Rules
A policy bundle signature is temporally valid only if all conditions hold:

1. `signer.active == true`
2. `valid_from <= issued_at < valid_until`
3. `issued_at <= evaluation_epoch`
4. `registry_epoch <= evaluation_epoch` (if registry metadata present; see below)
5. signer chain temporal validity holds at `issued_at` for every chain node:
   - `node.valid_from <= issued_at < node.valid_until`
   - `node.active == true`

No alternative inequalities are allowed.

## Revocation Model
1. Revocation is encoded only through signer registry version updates.
2. Revocation state MUST be represented in the sealed registry artifact (e.g., `active=false` and/or shortened `valid_until`).
3. External revocation lists are forbidden.
4. Runtime MUST validate only against the run-pinned signer registry artifact.

## Replay Protection
1. Replay acceptance is bounded by signer validity window and evaluation epoch.
2. Any signature with `issued_at >= valid_until` is invalid.
3. Any signature with `issued_at > evaluation_epoch` is invalid.
4. Indefinite reuse is forbidden by finite signer validity windows and run-pinned evaluation epoch.

## Registry Version Binding
Signer registry artifact MUST include:
- `registry_version` (string, mandatory)
- `registry_epoch` (integer UNIX epoch seconds, UTC, mandatory)

Binding rules:
1. Run MUST pin one registry version and digest.
2. Validation MUST use only pinned registry artifact.
3. `registry_epoch <= evaluation_epoch` MUST hold.
4. Signature validation MUST NOT mix signer data across registry versions.

## Failure Semantics
Validation MUST FAIL-CLOSED on any of the following:
1. missing `evaluation_epoch`
2. missing `issued_at`
3. non-integer time fields
4. invalid temporal ordering (`valid_from >= valid_until`)
5. signer expired at `issued_at`
6. signature issued in future (`issued_at > evaluation_epoch`)
7. missing registry version metadata
8. registry epoch after evaluation epoch
9. any chain node temporally invalid

No fallback, no partial acceptance, no warning-only mode.

## Time Representation
1. All temporal fields use UNIX epoch **seconds**.
2. Integer-only representation is mandatory.
3. UTC is mandatory.
4. Milliseconds, floats, timezone-offset strings, and wall-clock conversions are forbidden.

## Test Scenarios
1. Valid case:
   - `valid_from=1700000000`, `valid_until=1800000000`, `issued_at=1710000000`, `evaluation_epoch=1715000000` -> PASS
2. Expired signer:
   - `issued_at=1810000000`, `valid_until=1800000000` -> FAIL
3. Future signature:
   - `issued_at=1720000000`, `evaluation_epoch=1715000000` -> FAIL
4. Replay attempt:
   - old signature reused with `issued_at` outside current signer validity or after signer revocation window in pinned registry -> FAIL

## Final Statement
Temporal signer authority and signature validity are determined exclusively by immutable run inputs (`evaluation_epoch` and pinned signer registry version), integer UTC epoch rules, and fail-closed inequalities; no external time source or mutable runtime state is permitted.
