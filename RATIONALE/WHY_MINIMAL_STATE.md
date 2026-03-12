# WHY MINIMAL STATE

This document explains another core design decision of Boundary Protocol:

> Boundary Protocol adopts a **minimal-state architecture**.

Instead of maintaining large mutable global states, the protocol records events and observations while keeping persistent state as small as possible.

---

# Traditional State-Centric Systems

Many systems rely on a central mutable state:

- databases store the "current truth"
- configuration systems overwrite previous configurations
- infrastructure definitions replace previous versions

While effective for operational control, this approach often loses important historical information.

When state is overwritten, it becomes difficult to answer:

- what the previous state was
- how the state changed
- who triggered the change

For accountability purposes, this loss of history is problematic.

---

# State vs History

Boundary Protocol separates two concepts:

**State**

The condition of a system at a specific moment.

**History**

The chain of events that explains how the system reached that condition.

Traditional systems emphasize state.

Boundary Protocol emphasizes **history**.

---

# State as an Observation Artifact

Within the protocol, state is treated as an **observed artifact**, not the primary structure of the system.

A state record simply answers:

- what was observed
- who observed it
- when it was observed

The protocol does not attempt to maintain a canonical global state of the system.

---

# Benefits of Minimal State

### Reduced Ambiguity

When state is minimized and events are preserved, the system history becomes easier to reconstruct.

### Deterministic Verification

Verifiers can rely on event history and evidence artifacts rather than attempting to infer meaning from a mutable state snapshot.

### Distributed Compatibility

Minimal state reduces the need for centralized coordination between independent systems.

### Trace Durability

Event chains remain meaningful even if the underlying systems evolve.

---

# Interaction with Event-First Design

Minimal state works together with the protocol's **event-first architecture**.

Events describe actions and observations.

State records provide snapshots that may be referenced by those events.

The protocol therefore preserves both:

- the sequence of actions
- the conditions observed during those actions

---

# Why This Matters for Autonomous Systems

Autonomous systems often operate in dynamic environments where:

- state changes frequently
- decisions are distributed
- observations may come from multiple actors

Maintaining a single authoritative state can become difficult or impossible.

By minimizing state and focusing on events and observations, Boundary Protocol remains compatible with these environments.

---

# Summary

Boundary Protocol minimizes persistent state to preserve the integrity of historical traces.

State is treated as an observation artifact, while the primary structure of the protocol remains the chain of events that records system behavior and accountability.