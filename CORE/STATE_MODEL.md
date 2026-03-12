# State Model

## 1. Purpose

The State model defines how observable system conditions are represented within the Boundary Protocol.

A **State** represents the observable condition of a system or resource at a specific moment in time. It provides the evidence required to evaluate whether an intent has been realized by a transition.

State is the third element of the protocol's causal chain.

---

## 2. Core Definition

A **State** is a verifiable snapshot describing the observable condition of a subject at a specific point in time.

Conceptually:

```
State = observable condition of a subject
```

A state answers the question:

> What actually happened?

Examples:

- Service `A` running version `v2`
- TLS enforcement enabled
- Credential set rotated

---

## 3. Minimal Protocol State

Boundary Protocol intentionally defines only a **minimal protocol state**.

The protocol does not attempt to model the full internal state of a system.

Instead it defines only the information required to:

- observe a condition
- verify a result
- compare expected and actual outcomes

This allows the protocol to remain implementation-neutral.

---

## 4. State Characteristics

### Observability

State must be observable from outside the executing system.

Verification must be possible without relying on internal execution logic.

### Snapshot-Based

A state represents a snapshot at a particular moment.

States do not mutate. Instead, new states represent later observations.

### Verifiability

A state must contain enough information to allow independent verification.

### Content Addressing

States may be identified using deterministic identifiers.

```
state_id = hash(state_document)
```

This enables cross-system verification.

---

## 5. State Structure

A minimal conceptual state structure includes:

```
state
├─ state_id
├─ subject
├─ attributes
├─ observed_by
├─ observed_at
└─ evidence
```

### subject

Identifies the system resource whose condition is being described.

Example:

```
subject:
  system: deployment
  resource: serviceA
```

### attributes

Represents observable properties of the subject.

Example:

```
attributes:
  version: v2
  replicas: 3
```

### observed_by

Identity of the actor or system that produced the observation.

Observers may include:

- monitoring systems
- verification engines
- services
- humans

### observed_at

Timestamp representing when the observation occurred.

### evidence

Optional supporting data used to justify the observation.

Examples may include:

- logs
- system responses
- metrics

---

## 6. State and Transition Relationship

Transitions may reference states to describe system movement.

Conceptually:

```
from_state → transition → to_state
```

Where:

- `from_state` describes the condition before execution
- `to_state` describes the condition after execution

This allows systems to reconstruct observable changes.

---

## 7. State and Verification

Verification compares the expected result of an intent with the observed state.

Conceptually:

```
desired_state
vs
observed_state
```

Possible evaluation outcomes include:

- PASS
- FAIL
- UNKNOWN

Verification engines determine the evaluation logic.

The protocol itself only defines the structure of the observable state.

---

## 8. Multiple Observations

A system may produce multiple state observations over time.

Example:

```
state_1 → before transition
state_2 → after transition
state_3 → later verification
```

These observations allow verification engines to analyze system evolution.

---

## 9. Distributed Observation

State observations may originate from multiple observers.

The protocol does not assume a single trusted observer.

Multiple independent observations may coexist.

This supports distributed verification models.

---

## 10. Summary

The State model represents observable system conditions used to verify actions.

Key properties:

- observable
- snapshot-based
- verifiable
- content-addressed

Within the core protocol structure:

```
Intent
   ↓
Transition
   ↓
State
```

State provides the evidence necessary to evaluate whether intents were successfully realized.

