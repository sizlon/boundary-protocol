# Chain Integrity Rules

## 1. Purpose

The Chain Integrity Rules define how Boundary Protocol artifacts maintain tamper‑evident history.

Because the protocol records actions, observations, evaluations, and disputes across distributed systems, it must provide a minimal mechanism to detect whether historical records have been altered.

These rules define how artifacts reference each other cryptographically so that the protocol history forms a **verifiable integrity chain**.

---

## 2. Integrity Chain Principle

Every Boundary Protocol artifact should be verifiably connected to the artifacts it depends on.

Conceptually:

```
Intent
   ↓
Transition Event
   ↓
State
   ↓
Attestation
   ↓
Verdict
   ↓
Dispute
```

Each artifact references previous artifacts and includes their identifiers or hashes.

This creates a **linked verification chain**.

---

## 3. Artifact Identity

Each artifact must have a deterministic identifier.

Typical implementations derive this identifier from the artifact contents.

Example:

```
artifact_id = hash(serialized_artifact)
```

This ensures that any modification of the artifact changes its identifier.

---

## 4. Reference Integrity

When an artifact references another artifact, it must reference the target artifact by identifier.

Example:

```
transition_event.intent_ref → intent_id
verdict.transition_ref → transition_event_id
attestation.state_ref → state_id
```

Because identifiers are derived from artifact content, reference chains provide tamper detection.

---

## 5. Hash Chain Construction

Systems may optionally strengthen integrity guarantees by constructing explicit hash chains.

Example:

```
current_hash = hash(
    artifact_content
    + previous_hash
)
```

This creates a sequential integrity chain similar to append‑only logs.

The protocol does not mandate a single chaining method but allows implementations to use one where appropriate.

---

## 6. Immutable Artifacts

Artifacts must be immutable after publication.

If corrections are required, new artifacts must be created that reference the previous artifacts.

Example:

```
verdict_supersession
```

This rule ensures that the protocol history remains auditable.

---

## 7. Integrity Verification

Systems verifying protocol history should perform the following checks:

1. Artifact identifiers match artifact content hashes.
2. Referenced artifacts exist.
3. Reference chains are consistent.
4. No artifact content has been modified.

If any of these checks fail, the chain integrity is considered compromised.

---

## 8. Distributed Integrity

Because artifacts may be created by independent systems, integrity must not rely on a single authority.

Instead, integrity arises from:

- cryptographic identifiers
- reference chains
- independent verification

Multiple systems may independently validate the same artifact chain.

---

## 9. Integrity and Accountability

The integrity chain enables verifiable accountability.

By preserving a tamper‑evident history, systems can reconstruct:

- what actions were declared
- what transitions occurred
- what observations were made
- what evaluations were produced
- where disagreements arose

This property allows Boundary Protocol histories to be audited across system boundaries.

---

## 10. Summary

The Chain Integrity Rules ensure that protocol histories are tamper‑evident and verifiable.

Key properties:

- deterministic artifact identifiers
- reference‑based integrity
- optional hash chaining
- immutable artifacts
- distributed verification

Together, these rules allow Boundary Protocol histories to remain trustworthy even when recorded across multiple independent systems.

