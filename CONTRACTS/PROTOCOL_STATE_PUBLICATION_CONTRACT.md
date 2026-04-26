# Protocol State Publication Contract

## 1. Purpose

The Protocol State Publication Contract defines the sealed runtime/profile
artifact emitted after verdict and attestation, with an optional dispute
reference when a later dispute publication exists.

This contract is intentionally narrow. It does not define state transitions,
resolution logic, or a general state machine.

It defines the publication-layer state snapshot that summarizes the final run
outcome in protocol-facing terms.

---

## 2. Core Definition

A **Protocol State Publication** is a runtime/profile artifact that records
the published state of a sealed run.

Conceptually:

```text
Protocol State Publication = published run-level state summary
```

This artifact answers the question:

> what final published state does the sealed run have

---

## 3. Contract Goals

The state publication contract ensures that final state summaries are:

- explicit
- deterministic
- attributable
- fail-closed when malformed

The contract does not define state evolution. It only defines the final
publication shape.

---

## 4. Minimal Publication Structure

A minimal protocol state publication contains the following fields:

```text
protocol_state
├─ state_id
├─ intent_ref
├─ transition_ref
├─ subject_state
├─ state_code
├─ observed_at
└─ evidence_refs
```

An optional dispute publication may add:

- `dispute_ref`

---

## 5. Field Definitions

### state_id

Deterministic identifier of the final state publication artifact.

### intent_ref

Reference to the published intent identifier for the sealed run.

### transition_ref

Reference to the published transition identifier for the sealed run.

### subject_state

Runtime/profile state subject summary.

### state_code

Final run-level state code.

For the first slice, supported values are:

- `VERIFIED`
- `FAILED`
- `UNKNOWN`

### observed_at

Timestamp indicating when the final state was published.

### evidence_refs

Sealed artifact references that support the final state publication.

### dispute_ref

Optional reference to the published dispute artifact when a later dispute
publication exists.

---

## 6. Ownership Boundary

This artifact is publication-layer runtime/profile output.

It is not a direct core state object from `CORE/STATE_MODEL.md`.

The publication artifact summarizes the outcome of a sealed run after verdict
and attestation have been emitted.

---

## 7. Publication Rule

The first slice follows these rules:

1. `protocol_state.json` is always emitted
2. it must derive only from already-published runtime/profile artifacts
3. if a dispute publication exists, it must be linked through `dispute_ref`
4. verdict `PASS` maps to `VERIFIED`
5. verdict `FAIL` maps to `FAILED`
6. verdict `UNKNOWN` maps to `UNKNOWN`
7. `evidence_refs` must reference the published runtime/profile artifacts used
   to derive the state

---

## 8. Relationship To Other Artifacts

The publication chain is:

```text
protocol_verdict.json
protocol_attestation.json
protocol_dispute.json (optional)
→ protocol_state.json
```

The state publication summarizes the final published status of the run.
The dispute publication is an overlay on top of that status, not a separate
state code.

---

## 9. Immutability

Protocol state publication records are immutable once emitted.

Corrections must be expressed as new publication records.
