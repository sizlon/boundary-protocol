# Protocol Dispute Publication Contract

## 1. Purpose

The Protocol Dispute Publication Contract defines the sealed runtime/profile
artifact used to publish a run-level dispute record when explicit dispute input
is present.

This contract is intentionally narrow. It does not define dispute resolution,
appeal workflows, or dispute timelines.

It defines the publication-layer form of a dispute record that is emitted by an
executor after a sealed run has already produced a verdict and an attestation.

---

## 2. Core Definition

A **Protocol Dispute Publication** is a runtime/profile publication artifact
that records that a sealed run's published verdict or attestation is being
formally disputed.

Conceptually:

```text
Protocol Dispute Publication = sealed dispute publication for a run-level artifact
```

This artifact answers the question:

> what sealed runtime artifact is being disputed, by whom, and why

---

## 3. Contract Goals

The dispute publication contract ensures that dispute records are:

- explicit
- attributable
- deterministic
- fail-closed when malformed

The contract does not define adjudication. It only defines the publication
shape for a sealed dispute record.

---

## 4. Minimal Publication Structure

A minimal protocol dispute publication contains the following fields:

```text
protocol_dispute
├─ dispute_id
├─ intent_ref
├─ transition_ref
├─ raised_by
├─ raised_at
├─ reason_code
├─ subject_ref
└─ supporting_evidence_refs
```

---

## 5. Field Definitions

### dispute_id

Deterministic identifier of the dispute publication artifact.

### intent_ref

Reference to the published intent identifier for the sealed run.

### transition_ref

Reference to the published transition identifier for the sealed run.

### raised_by

Identity raising the dispute publication.

### raised_at

Timestamp indicating when the dispute was raised.

### reason_code

Machine-readable reason label for the dispute publication.

### subject_ref

Reference to the published artifact being disputed.

For the first slice, supported values are:

- `protocol_verdict.json`
- `protocol_attestation.json`

### supporting_evidence_refs

Optional sealed artifact references that support the dispute publication.

---

## 6. Ownership Boundary

This artifact is publication-layer runtime/profile output.

It is not the same thing as the generic protocol dispute core object defined
elsewhere in this repository.

The publication artifact is produced after the runtime has already published
verdict and attestation artifacts.

---

## 7. Publication Rule

The first slice follows these rules:

1. dispute input is optional
2. if absent, no dispute publication is emitted
3. if present, dispute input must validate fail-closed
4. `intent_ref` must match the published verdict `intent_ref`
5. `transition_ref` must match the published verdict `transition_ref`
6. `subject_ref` must target a published protocol artifact
7. `supporting_evidence_refs`, when present, must reference sealed runtime
   artifacts only

---

## 8. Relationship To Other Artifacts

The publication chain is:

```text
protocol_verdict.json
protocol_attestation.json
→ protocol_dispute.json
→ protocol_state.json
```

The dispute publication may refer to either the verdict or the attestation as
its subject.
It does not rewrite the published `protocol_state.json` state code; it only
adds a separate dispute publication reference.

---

## 9. Immutability

Protocol dispute publication records are immutable once emitted.

Corrections must be expressed as new publication records.
