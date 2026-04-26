# Protocol Author Trust Contract

## 1. Purpose

The Protocol Author Trust Contract defines how authored protocol artifacts are
bound to accountable author identities in the first sealed-run authoring slice.

This contract is intentionally narrow. It does not define a general trust
framework, signer hierarchy, or cryptographic validation policy.

It defines the minimal companion records used to tie authored protocol
artifacts to the identities that declared them.

---

## 2. Core Definition

An **author trust companion** is a structured record that names the authored
artifact, the responsible author identity, and the companion signature record
used to support that authorship claim.

Conceptually:

```text
Author Trust Companion = authored artifact binding record
```

The companion answers the question:

> who is accountable for authoring this protocol artifact

---

## 3. Contract Goals

The author trust contract ensures that authored protocol artifacts are:

- attributable
- companioned
- deterministic
- fail-closed when malformed

This contract does not define the cryptographic verification procedure. It only
defines the structure of the authored companion records.

---

## 4. Minimal Companion Structure

The first sealed-run authoring slice uses two companion record families:

- `protocol_intent_author.json`
- `protocol_transition_author.json`

Each companion references a signature envelope:

- `protocol_intent_signature_envelope.json`
- `protocol_transition_signature_envelope.json`

The companion structure is:

```text
protocol_*_author
├─ version
├─ artifact_ref
├─ author_id
├─ signature_envelope_ref
└─ declared_at
```

The signature envelope structure is:

```text
protocol_*_signature_envelope
├─ version
├─ artifact_ref
├─ signer_id
├─ signature_algorithm
├─ signature
├─ signed_at
└─ key_hint
```

---

## 5. Field Definitions

### version

Schema version for the companion slice.

The first supported value is:

```text
version: 1
```

### artifact_ref

Reference to the authored protocol artifact being companioned.

For the first slice, supported values are:

- `protocol_intent.json`
- `protocol_transition_event.json`

### author_id

Identity responsible for declaring authorship of the companioned artifact.

### signature_envelope_ref

Reference to the companion signature envelope artifact.

### declared_at

Timestamp indicating when authorship was declared.

### signer_id

Identity represented by the signature envelope.

### signature_algorithm

Algorithm used to produce the signature envelope.

### signature

Signature material associated with the authored artifact.

### signed_at

Timestamp indicating when the signature envelope was produced.

### key_hint

Optional hint that may help identify the signing key.

---

## 6. Ownership Boundary

The authored protocol artifact remains the primary authored record.

The trust companion and signature envelope are protocol-side supporting
artifacts. They record who stands behind the authored record, but they do not
create new semantic meaning on their own.

---

## 7. Compatibility Rule

The first sealed-run authoring slice allows bridge-only compatibility when an
authored artifact is absent.

When both an authored artifact and its author companion are present, they must
refer to the same authored object.

If a companion exists but points to the wrong artifact, execution must fail
closed in implementations that admit the slice.

---

## 8. Relationship To Other Artifacts

The first supported authoring chains are:

```text
protocol_intent.json
→ protocol_intent_author.json
→ protocol_intent_signature_envelope.json
```

```text
protocol_transition_event.json
→ protocol_transition_author.json
→ protocol_transition_signature_envelope.json
```

These chains support sealed-run authoring while preserving compatibility with
bridge-only runtime inputs.

---

## 9. Immutability

Author trust companions are immutable once authored.

Corrections must be expressed as new companion records, not edits.

