# Policy Bundle Signer Authority Chain Spec

## Purpose

This document defines how signer authority is validated across a delegation chain
and how scope is inherited in a country-only policy bundle model.

The goal is to ensure:

- no signer can exceed its parent authority
- authority is explicitly declared
- validation is deterministic
- execution remains fail-closed

---

## Core Principle

A delegated signer may only operate within a subset of its parent’s authority.

Authority must never expand along the chain.

---

## Signer Types

### Root Signer

- type: root
- May omit scope
- If omitted, it is treated as having full authority

---

### Delegated Signer

- type: delegated
- MUST include:
  - parent
  - active = true
  - scope

---

## Scope Model (Country-Only)

scope:
  allowed_bundles: ["org-a-*"]
  allowed_plans: ["pro"]
  countries: ["KR"]

### Fields

- allowed_bundles: pattern list
- allowed_plans: explicit plan list
- countries: ISO 3166-1 alpha-2 codes

---

## Chain Validation Rules

For a given signer:

1. Signer MUST exist
2. Signer MUST be active
3. If delegated:
   - parent MUST exist
   - parent MUST be active
4. No cycles allowed in parent chain

Failure of any rule → FAIL (hard stop)

---

## Scope Presence Rules

- Root signer:
  - scope MAY be omitted

- Delegated signer:
  - scope MUST exist
  - MUST include:
    - allowed_bundles
    - allowed_plans
    - countries

Missing any → FAIL

---

## Scope Inheritance (No Widening)

Effective authority is computed as the intersection across the chain.

---

### 1. Countries

Rule:

child.countries ⊆ parent.countries

Effective scope:

effective_countries = intersection(all countries in chain)

Failure:

- child introduces country not in parent → FAIL
- intersection becomes empty → FAIL

---

### 2. Allowed Plans

Rule:

child.allowed_plans ⊆ parent.allowed_plans

Effective scope:

effective_plans = intersection(all allowed_plans in chain)

Failure:

- child introduces plan not in parent → FAIL
- intersection becomes empty → FAIL

---

### 3. Allowed Bundles

Rule:

A bundle must satisfy ALL patterns in the chain.

bundle_id matches:
  child.allowed_bundles
AND
  parent.allowed_bundles
AND
  ...

Root signer without scope imposes no restriction.

Failure:

- bundle does not match any level → FAIL

---

## Effective Scope Derivation

Final scope used for validation:

effective_scope = {
  countries: intersection(chain.countries),
  allowed_plans: intersection(chain.allowed_plans),
  allowed_bundles: cumulative pattern constraint
}

No inference, no expansion.

---

## Bundle Validation Using Effective Scope

A bundle is valid only if:

1. bundle.country exists
2. bundle.country ∈ effective_countries
3. bundle_id matches effective bundle patterns
4. bundle-exposed plans ⊆ effective_plans

Any violation → FAIL (fail-closed)

---

## Forbidden Behaviors (Hard Invariants)

The following are strictly prohibited:

- Child widening parent scope
- Implicit inheritance when scope is missing
- Jurisdiction inference
- Pattern bypass via naming tricks
- Use of scope_groups for execution logic
- Fallback or partial acceptance

---

## Failure Cases

### 1. Country widening

parent.countries = ["KR"]
child.countries = ["US"]

→ FAIL

---

### 2. Plan widening

parent.allowed_plans = ["basic"]
child.allowed_plans = ["pro"]

→ FAIL

---

### 3. Empty intersection

parent.countries = ["KR"]
child.countries = ["JP"]

→ intersection = ∅ → FAIL

---

### 4. Bundle pattern mismatch

parent: org-a-*
child: org-a-kr-*
bundle: org-b-123

→ FAIL

---

### 5. Inactive parent

parent.active = false

→ FAIL

---

### 6. Cycle

A → B → C → A

→ FAIL

---

## Determinism Guarantees

- No runtime inference
- No document-based routing
- No heuristic matching
- No fallback

Validation depends only on:

- signer chain
- declared scope
- bundle metadata

---

## Summary

- Authority is hierarchical and restrictive
- Scope is explicitly declared and intersected
- Validation is deterministic and fail-closed
- Country-only model simplifies governance without loss of control

---

## Status

READY

