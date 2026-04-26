# Protocol Intent Contract

## 1. Purpose

The Protocol Intent Contract defines the first protocol-native intent artifact
used for sealed-run verification.

This contract is intentionally narrow. It does not define a general intent
taxonomy, negotiation workflow, or amendment lifecycle.

It defines a single authored intent shape that higher layers may use to bind a
sealed runtime run to a protocol-native intent record.

---

## 2. Core Definition

A **Protocol Intent** is an immutable authored record declaring that a sealed
run should be verified under a specific protocol profile.

Conceptually:

```text
Protocol Intent = authored verification intent for a sealed run
```

This artifact answers the question:

> what sealed runtime outcome should be verified under which protocol profile

---

## 3. Contract Goals

The Protocol Intent Contract ensures that authored verification intent is:

- explicit
- deterministic
- attributable
- evidence-linked

The contract does not define how execution occurs. It only defines how the
authored intent is recorded.

---

## 4. Minimal Protocol Intent Structure

A minimal protocol intent contains the following fields:

```text
protocol_intent
├─ version
├─ intent_id
├─ intent_type
├─ issuer
├─ issued_at
├─ target_profile
├─ target_subject
├─ requested_outcome
└─ evidence_refs
```

---

## 5. Field Definitions

### version

Schema version for this authored intent slice.

The first supported value is:

```text
version: 1
```

### intent_id

Deterministic identifier for the authored intent record.

### intent_type

Stable narrow intent type for the first slice.

The only supported value in this slice is:

```text
sealed_run_verification
```

### issuer

Identity responsible for authoring the intent.

### issued_at

Timestamp indicating when the intent was authored.

### target_profile

Profile or runtime contract scope under which the run should be verified.

### target_subject

Stable description of the sealed subject being verified.

### requested_outcome

Minimal requested verification outcome statement.

### evidence_refs

Evidence references supporting intent authoring.

These references are optional in broader protocol semantics, but the first
sealed-run authoring slice requires them to be present and non-empty.

---

## 6. Ownership Boundary

Protocol intent authoring is protocol-owned.

Executors may admit and validate the artifact, but they must not invent its
meaning at runtime.

Runtime bridge helpers such as `protocol_intent_ref.json` may consume the
authored intent, but they do not replace it as the authored authority source.

---

## 7. Relationship To Other Artifacts

The first supported relationship is:

```text
protocol_intent.json -> protocol_intent_ref.json -> protocol_verdict.json
```

The reference helper may be used for compatibility, but the authored intent is
the primary protocol-native source of `intent_ref` once it is present and
valid.

---

## 8. Immutability

Protocol intents are immutable once authored.

Any change must be expressed as a new authored intent record.

