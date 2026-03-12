# Event Model

## 1. Purpose

The Event Model defines how Boundary Protocol artifacts are created and connected through events.

While the protocol defines core objects (intent, transition, state, attestation, verdict, dispute), the historical record of the protocol is represented through **events** that create or reference these objects.

This model allows the protocol to maintain a verifiable, append-only history of actions, observations, evaluations, and disagreements.

---

## 2. Event‑First Principle

Boundary Protocol histories are built from events.

Conceptually:

```
event → artifact creation or reference
```

An event records that something happened and identifies the artifact produced or referenced as a result.

This allows distributed systems to reconstruct the protocol history by replaying events.

---

## 3. Core Event Types

The protocol defines six primary event types.

```
intent_declared
transition_attempted
state_observed
attestation_published
verdict_issued
dispute_raised
```

Each event corresponds to the creation of a protocol artifact.

---

## 4. Event → Artifact Mapping

Each event produces a corresponding artifact.

```
intent_declared      → intent
transition_attempted → transition_event
state_observed       → state
attestation_published→ attestation
verdict_issued       → verdict
dispute_raised       → dispute
```

Artifacts provide stable references, while events provide historical ordering.

---

## 5. Event Structure

A minimal event record contains the following fields:

```
event
├─ event_id
├─ event_type
├─ artifact_ref
├─ actor
├─ timestamp
└─ causal_refs[]
```

### event_id

Deterministic identifier for the event record.

```
event_id = hash(event_document)
```

### event_type

One of the protocol‑defined event types.

### artifact_ref

Reference to the artifact created or referenced by the event.

### actor

Identity responsible for emitting the event.

### timestamp

Time when the event was recorded.

### causal_refs

References to events that causally precede the current event.

These references allow reconstruction of the protocol’s causal graph.

---

## 6. Event Causality

Events form a causal chain.

Example:

```
intent_declared
      ↓
transition_attempted
      ↓
state_observed
      ↓
attestation_published
      ↓
verdict_issued
      ↓
dispute_raised
```

Each event references the event(s) that logically precede it.

This enables reconstruction of execution and verification history.

---

## 7. Event Immutability

Events are immutable once published.

Protocol histories must be append‑only.

If corrections are required, new events must reference the earlier events rather than modifying them.

---

## 8. Distributed Event Emission

Events may be emitted by independent systems.

Examples include:

- execution systems
- observation systems
- verification engines
- audit systems

Because of this, the protocol does not require a global ordering authority.

Ordering is reconstructed through causal references.

---

## 9. Event Replay

Systems may reconstruct protocol history by replaying events.

Event replay allows systems to rebuild:

- execution sequences
- observation history
- evidence chains
- verification outcomes
- dispute records

This property enables distributed verification and auditing.

---

## 10. Summary

The Event Model defines how Boundary Protocol histories are constructed.

Key properties:

- event‑based history
- artifact anchoring
- causal event chains
- append‑only records
- distributed event emission

Artifacts provide stable meaning, while events provide historical context.

Together, they allow Boundary Protocol systems to record and verify accountability across system boundaries.

