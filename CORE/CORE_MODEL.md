# Core Model

## 1. Purpose

The Boundary Protocol defines a minimal, implementation‑neutral protocol for describing **intent**, recording **transitions**, and verifying **observable state** across system boundaries.

The protocol is intentionally small. It defines only the minimal primitives required to make actions **verifiable, attributable, and auditable**.

Boundary Protocol does not define application logic, workflows, orchestration, or runtime execution. Those belong to platforms and implementations built on top of the protocol.

---

## 2. Core Objects

Boundary Protocol is built on **three core objects**:

- **Intent**
- **Transition**
- **State**

Runtime traces record these artifacts through Events.

These three objects form the minimal structure required to represent:

1. What should happen
2. What was attempted
3. What actually happened

### Summary

| Object | Role |
|------|------|
| Intent | Declares the desired outcome |
| Transition | Records an attempt to realize an intent |
| State | Represents the observable condition of a system |

---

## 3. Intent

An **Intent** declares a desired condition or outcome.

Characteristics:

- Immutable once created
- Identified by a content‑addressed identifier
- Represents a desired state or action

An intent answers the question:

> **What should happen?**

Examples:

- Deploy service version `v2`
- Rotate credentials
- Enable TLS

An intent does not guarantee execution. It only declares a desired condition.

---

## 4. Transition

A **Transition** records an attempt to realize an intent.

Characteristics:

- References an intent
- Represents an execution attempt
- May succeed or fail
- Can occur multiple times for a single intent

A transition answers the question:

> **What was attempted?**

Transitions create an **append‑only execution history**.

Example lifecycle:

Intent

→ Transition Attempt #1 (fail)

→ Transition Attempt #2 (fail)

→ Transition Attempt #3 (success)

---

## 5. State

A **State** represents the observable condition of a system at a specific moment.

Boundary Protocol defines only a **minimal protocol state**, not a full system model.

Characteristics:

- Observable
- Verifiable
- Snapshot‑based

State answers the question:

> **What actually happened?**

The protocol only requires enough structure to compare **desired state** and **observed state**.

Detailed domain state models are defined by implementations.

---

## 6. Verification

Verification evaluates whether a transition successfully realized an intent.

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

The key comparison is:

```
desired_state
vs
observed_state
```

Possible outcomes include:

- PASS
- FAIL
- UNKNOWN

The exact evaluation logic is defined by verification engines implementing the protocol.

---

## 7. Relationship of the Core Objects

The three objects form a minimal causal chain:

```
Intent
   ↓
Transition
   ↓
State
```

Where:

- Intent defines the desired outcome
- Transition records execution attempts
- State captures observable results

This structure enables verifiable histories of actions across system boundaries.

---

## 8. Protocol Scope

Boundary Protocol intentionally defines only a small set of primitives.

The protocol **does not define**:

- Approval workflows
- Organizational governance
- Execution engines
- Scheduling systems
- Orchestration logic

Those concerns belong to higher‑level systems built on the protocol.

---

## 9. Protocol Layering

Boundary Protocol sits at a foundational layer:

```
Applications
    ↑
Platforms / Automation Systems
    ↑
Verification Engines
    ↑
Boundary Protocol
```

Implementations may include governance systems, automation agents, or verification engines.

The protocol itself remains neutral and implementation‑independent.

---

## 10. Design Principles

The design of the core model follows these principles:

### Minimality

The protocol defines only the primitives necessary for verifiable action histories.

### Immutability

Core artifacts such as intents and transitions are append‑only.

### Verifiability

All actions can be independently verified.

### Implementation Neutrality

The protocol does not assume any specific runtime, platform, or vendor.

---

## 11. Summary

Boundary Protocol is built on three primitives:

- **Intent** — declares what should happen
- **Transition** — records what was attempted
- **State** — represents what actually happened

Together they enable a verifiable model for actions across system boundaries.

