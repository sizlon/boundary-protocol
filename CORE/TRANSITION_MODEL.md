# Transition Model

## 1. Purpose

The Transition model defines how attempts to realize intents are recorded within the Boundary Protocol.

A **Transition** represents an execution attempt associated with an intent. It records that a system or actor attempted to realize a declared intent.

Transitions provide the **execution history** of the protocol.

---

## 2. Core Definition

A **Transition** is an immutable record of an attempt to realize a specific intent.

Conceptually:

```
Transition = attempt to realize intent
```

A transition answers the question:

> What was attempted?

Examples:

- Deployment attempt for service version `v2`
- Credential rotation execution
- TLS enablement procedure

A transition does not guarantee success. It only records that an attempt occurred.

---

## 3. Transition Characteristics

### Immutability

Transitions are append-only records.

Once recorded, a transition cannot be modified.

### Intent Reference

Every transition references the intent it attempts to realize.

```
transition.intent_ref → intent_id
```

### Multiple Attempts

A single intent may have multiple transition attempts.

Example:

```
intent: deploy_v2

transition_1 → fail
transition_2 → fail
transition_3 → success
```

This enables transparent execution histories.

---

## 4. Transition Structure

A minimal conceptual transition structure includes:

```
transition
├─ event_id
├─ intent_ref
├─ actor
├─ attempted_at
├─ outcome
├─ from_state
└─ to_state
```

### event_id

Content-addressed identifier of the transition event artifact.

```
event_id = hash(transition_event_document)
```

### intent_ref

Reference to the intent being attempted.

### actor

Identity of the system or actor performing the transition.

Actors may include:

- humans
- services
- automated systems
- agents

### attempted_at

Timestamp indicating when the transition attempt occurred.

### outcome

The result of the attempt.

Possible values include:

- success
- fail

### from_state

Optional reference to the observed system state before execution.

### to_state

Reference to the observed system state after execution.

---

## 5. Transition Outcomes

A transition may produce different results.

### Success

The transition successfully realizes the intent.

```
outcome: success
from_state ≠ to_state
```

### Failure

The transition attempt fails and the system state remains unchanged.

```
outcome: fail
from_state = to_state
```

Failure transitions are still recorded to preserve execution history.

---

## 6. Transition and State

Transitions connect intents with observable system states.

Conceptually:

```
Intent
   ↓
Transition
   ↓
Observed State
```

Transitions capture how systems move between states.

They do not define the internal mechanics of execution.

---

## 7. Append-Only History

Transitions create an append-only history for each intent.

Example:

```
intent
├─ transition_1
├─ transition_2
└─ transition_3
```

This allows systems to reconstruct execution attempts and verify outcomes.

---

## 8. Distributed Execution

Transitions may originate from different actors or systems.

The protocol does not require a single centralized executor.

Multiple actors may attempt transitions for the same intent.

This enables distributed verification and execution environments.

---

## 9. Verification Relationship

Verification engines evaluate whether a transition satisfied the intent.

Conceptually:

```
Intent
   ↓
Transition
   ↓
Observed State
   ↓
Verification
```

Verification compares:

```
desired_state
vs
observed_state
```

The protocol records the transition but does not enforce evaluation logic.

---

## 10. Summary

The Transition model records attempts to realize intents.

Key properties:

- immutable
- append-only
- intent-referenced
- actor-attributed

Transitions form the execution history of the protocol.

Within the core model:

```
Intent
   ↓
Transition
   ↓
State
```

This structure enables transparent and verifiable action histories across system boundaries.
