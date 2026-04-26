# Dispute Contract

## 1. Purpose

The Dispute Contract defines how conflicts between verification results are formally recorded within the Boundary Protocol.

In a distributed verification environment, different evaluators may produce different verdicts for the same intent and transition. The Dispute Contract provides a minimal structure to record that such a conflict exists.

A dispute does not resolve the conflict. It only records that a disagreement has been formally recognized.

---

## 2. Core Definition

A **Dispute** is a structured record declaring that multiple verification results concerning the same subject are in conflict.

Conceptually:

```
Dispute = formal record of conflicting verification results
```

A dispute answers the question:

> Is there a recognized disagreement about the verification outcome?

---

## 3. Design Principles

The Dispute Contract follows several principles:

### Recognition, Not Resolution

The protocol records that a dispute exists but does not determine how it should be resolved.

### Immutability

Dispute records are append-only artifacts.

### Evidence Referencing

Disputes must reference the conflicting verdicts or attestations that triggered the disagreement.

### Implementation Neutrality

Resolution policies are defined by systems built on top of the protocol, not by the protocol itself.

---

## 4. Minimal Dispute Structure

A minimal dispute record contains the following fields:

```
dispute
├─ dispute_id
├─ subject_ref
├─ related_verdicts[]
├─ raised_by
├─ raised_at
├─ reason
└─ status
```

---

## 5. Field Definitions

### dispute_id

Deterministic identifier of the dispute artifact.

```
dispute_id = hash(dispute_document)
```

---

### subject_ref

Reference to the object being disputed.

Possible values may include:

- intent
- transition event
- verdict

Example:

```
subject_ref: transition_event_e2b1f8
```

---

### related_verdicts

List of verdict identifiers involved in the disagreement.

Example:

```
related_verdicts:
  - verdict_A
  - verdict_B
```

---

### raised_by

Identity that raised the dispute.

This may include:

- verification systems
- governance systems
- auditors
- human reviewers

---

### raised_at

Timestamp indicating when the dispute was raised.

Example:

```
raised_at: 2026-03-07T13:10:00Z
```

---

### reason

Explanation describing why the dispute was raised.

Example:

```
reason: "Conflicting verification results between verifier_A and verifier_B"
```

---

### status

Current status of the dispute record.

Typical values include:

- open
- closed
- superseded

Status does not imply how the dispute was resolved, only whether it remains active.

---

## 6. Example Dispute

Example dispute record:

```
dispute
  dispute_id: d91ab4...
  subject_ref: transition_event_e2b1f8
  related_verdicts:
    - verdict_A
    - verdict_B
  raised_by: audit_service
  raised_at: 2026-03-07T13:10:00Z
  reason: "Conflicting PASS/FAIL evaluations"
  status: open
```

---

## 7. Relationship to Verification

Disputes appear when verification results disagree.

```
Intent
   ↓
Transition Event
   ↓
State
   ↓
Attestation
   ↓
Verdict A
Verdict B
   ↓
Dispute
```

The dispute object records the presence of this disagreement.

---

## 8. Resolution Outside the Protocol

The Boundary Protocol does not define how disputes are resolved.

Resolution mechanisms may include:

- governance decisions
- additional verification
- arbitration systems
- human review

These mechanisms belong to higher-level systems.

The runtime/profile publication form of a dispute record is defined separately
in [PROTOCOL_DISPUTE_PUBLICATION_CONTRACT.md](/home/manny/Projects/sizlon/boundary-protocol/CONTRACTS/PROTOCOL_DISPUTE_PUBLICATION_CONTRACT.md).

---

## 9. Summary

The Dispute Contract defines how conflicts between verification results are recorded.

Key properties:

- conflict recognition
- verdict referencing
- append-only records
- implementation neutrality

Disputes extend the protocol's accountability model by ensuring that disagreements about verification outcomes are themselves part of the verifiable history.
