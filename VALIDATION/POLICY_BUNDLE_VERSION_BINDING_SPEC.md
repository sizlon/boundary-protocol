# POLICY_BUNDLE_VERSION_BINDING_SPEC

## Status
Normative (Binding)

## Purpose
Define the policy bundle version binding model so that signed bundles are bound to immutable policy semantics and cannot be reinterpreted under changed dependencies.

## Semantic Dependency Model
Policy semantics are the exact, ordered dependency set below:

1. Checklist definitions (all admitted checklist artifacts resolved by bundle policy)
2. Checklist schema
3. Evaluation operator model definition
4. Plan classification and mapping rules used by bundle policy:
   - plan-to-capability mapping artifact
   - capability-to-checklist mapping artifact
5. Scope model definitions used for executional binding:
   - signer scope semantics
   - scope field schema used by runtime

Only artifacts explicitly included in the bundle semantic dependency set contribute to meaning.

## Version Binding Model
Binding mechanism is **C (both explicit version fields and content digest binding)**.

Structural rule:
1. Version alone is insufficient because different content can share a label.
2. Digest alone is insufficient for governance lineage and compatibility policy.
3. Therefore, both MUST be present and both MUST validate.

## Binding Fields
Policy bundle manifest MUST include the following top-level fields:

1. `policy_version` (string, non-empty)
2. `semantic_digest` (string, lowercase hex SHA-256)
3. `semantic_dependencies` (non-empty array of dependency entries)

Each `semantic_dependencies[]` entry MUST include:
- `path` (string)
- `sha256` (64-char lowercase hex)
- `role` (string)

## Digest Definition
1. `semantic_digest` input is the canonical JSON representation of an ordered semantic dependency manifest object.
2. Canonicalization MUST follow `POLICY_BUNDLE_CANONICALIZATION_SPEC`.
3. Digest algorithm MUST be SHA-256.
4. Digest output format MUST be lowercase hexadecimal (64 chars).

Digest scope MUST include exactly:
1. `policy_version`
2. ordered `semantic_dependencies[]` entries (`path`, `sha256`, `role`)
3. no runtime-generated fields
4. no mutable repository references

Any semantic artifact affecting policy meaning that is absent from `semantic_dependencies[]` is prohibited.

## Validation Rules
A policy bundle is valid only if all conditions hold:

1. `policy_version` exists and matches expected policy lineage for the bundle reference.
2. All `semantic_dependencies[]` entries resolve to pinned runtime artifacts.
3. For each dependency, runtime-computed SHA-256 equals declared `sha256`.
4. Canonical semantic manifest digest equals `semantic_digest`.
5. No dependency is missing.
6. No undeclared semantic artifact is used.

## Runtime Binding
1. Runtime MUST load semantic dependencies only from pinned run artifacts under run input space.
2. Runtime MUST NOT load semantics from repository HEAD, mutable working tree files, or external URLs.
3. Runtime MUST record the resolved semantic dependency set and computed digest in run artifacts.
4. Semantic dependency resolution MUST be deterministic and closed over run inputs.

## Backward Compatibility
1. Older bundles MUST be validated against the semantic dependency set declared in their own manifest.
2. Older bundles MUST NOT be reinterpreted under newer semantic rules unless re-signed as a new bundle version.
3. Compatibility handling MUST preserve original `policy_version` and `semantic_digest` semantics.

## Failure Semantics
Validation MUST FAIL-CLOSED if any of the following occurs:

1. missing `policy_version`
2. missing `semantic_digest`
3. missing `semantic_dependencies`
4. semantic dependency missing at runtime
5. dependency SHA mismatch
6. semantic digest mismatch
7. version mismatch
8. undefined policy reference
9. use of undeclared semantic artifact

No fallback, partial acceptance, or reinterpretation is allowed.

## Test Scenarios
1. Valid bundle with matching semantics:
   - all dependencies present
   - all dependency hashes match
   - semantic digest matches
   - PASS

2. Semantic drift case:
   - one dependency content changed
   - dependency hash or semantic digest mismatch
   - FAIL

3. Missing dependency case:
   - declared dependency path unavailable in pinned runtime set
   - FAIL

## Final Statement
A signed policy bundle is semantically valid only when `policy_version`, `semantic_dependencies`, and `semantic_digest` jointly and deterministically bind execution to an immutable, pinned policy meaning set.
