# Causal Ordering Rules

## 1. Purpose

The Causal Ordering Rules define how protocol artifacts relate to one another in time and dependency.

Because Boundary Protocol records an append‑only history of events across distributed systems, it must define minimal rules that allow systems to reconstruct **causal relationships** between artifacts.

These rules ensure that histories remain **verifiable, interpretable, and logically consistent**.

---

## 2. Causal Chain Principle

Boundary Protocol artifacts form a causal chain of responsibility and observation.

Conceptually:

```
Intent
   ↓
Transition Event
   ↓
State Observation
   ↓
Attestation
   ↓
Verdict
   ↓
Dispute (optional)
```

Each artifact must reference the artifact(s) that logically precede it.

This referencing relationship forms the protocol's **causal graph**.

---

## 3. Minimal Ordering Guarantees

The protocol requires only minimal guarantees for ordering.

Systems must ensure that:

- references always point to existing artifacts
- artifacts are immutable
- causal relationships are explicitly declared

The protocol does **not require a global clock** or centralized ordering authority.

---

## 4. Intent Precedence Rule

A transition event must reference a previously declared intent.

```
transition_event.intent_ref → intent_id
```

This ensures that every execution attempt can be traced back to a declared intention.

---

## 5. Transition–State Relationship Rule

A transition event may reference states representing system conditions before and after execution.

```
from_state → transition_event → to_state
```

These references allow systems to understand how system conditions evolved.

---

## 6. Observation Dependency Rule

State observations must reference the subject being observed.

Observers may produce observations independently of transitions, but the observed subject must be identifiable.

Example:

```
state.subject → resource_identifier
```

---

## 7. Attestation Dependency Rule

Attestations must reference the state observation they assert.

```
attestation.subject_state → state_id
```

This ensures that attestations can always be traced to the observation they describe.

---

## 8. Verdict Dependency Rule

A verdict must reference both the intent and the transition event being evaluated.

```
verdict.intent_ref → intent_id
verdict.transition_ref → transition_event_id
```

Verdicts must also reference the evidence used during evaluation.

```
verdict.evidence_refs → attestation_ids
```

This creates a verifiable link between evaluation and evidence.

---

## 9. Dispute Dependency Rule

Disputes must reference the conflicting verification artifacts.

```
dispute.related_verdicts → verdict_ids
```

A dispute may also reference the transition or intent that the disagreement concerns.

---

## 10. Append‑Only History Rule

All protocol artifacts must be immutable and append‑only.

Artifacts must never be modified after publication.

Corrections or updates must be expressed through additional artifacts.

This rule guarantees that historical records remain auditable.

---

## 11. Distributed Ordering

Because the protocol operates in distributed environments, artifacts may be produced by multiple systems concurrently.

The protocol therefore does not require strict total ordering.

Instead, ordering is reconstructed using:

- artifact references
- timestamps
- causal dependencies

This allows systems to interpret histories without centralized coordination.

---

## 12. Summary

Causal ordering in the Boundary Protocol is defined by explicit references between artifacts rather than by a centralized timeline.

Key properties:

- dependency‑based ordering
- append‑only artifacts
- distributed compatibility

These rules allow independent systems to reconstruct and verify the history of actions, observations, and evaluations across system boundaries.

