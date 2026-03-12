# WHY EVENT-FIRST

This document explains another core design decision of Boundary Protocol:

> Boundary Protocol follows an **event-first architecture**.

Rather than modeling systems primarily through mutable state, the protocol records **discrete events** that describe what happened across system boundaries.

---

# Traditional System Modeling: State-Centric

Most software systems are designed around **state**.

In state-centric systems:

- the current state of a system is treated as the primary source of truth
- historical events are often secondary or discarded
- reconstruction of past behavior can be difficult or impossible

Examples include:

- database row updates
- configuration overwrites
- mutable infrastructure definitions

While state models work well for operational control, they are often insufficient for **accountability reconstruction**.

---

# The Accountability Problem

Accountability requires answering questions such as:

- What happened?
- When did it happen?
- Who performed the action?
- What evidence supports the action?

If systems only expose current state, many of these questions cannot be reliably answered.

State alone does not reveal **how the system arrived at that state**.

---

# Events as Historical Evidence

Boundary Protocol addresses this by treating **events as first-class objects**.

Events represent meaningful occurrences such as:

- intent declarations
- transition attempts
- state observations
- attestations
- verdicts
- disputes

Each event records a moment in the accountability history of a system interaction.

---

# Event Chains

Events are not isolated. They form **causal chains**.

For example:

```text
intent declared
    ↓
transition attempted
    ↓
state observed
    ↓
attestation published
    ↓
verdict issued
    ↓
dispute raised
```

This chain allows independent observers to reconstruct the sequence of actions and evaluations.

---

# Benefits of Event-First Design

An event-first architecture provides several advantages for accountability infrastructure.

### Historical Reconstruction

Events allow the full history of a system interaction to be reconstructed.

### Causality

Event chains make it possible to understand how outcomes emerged from earlier actions.

### Evidence Linking

Events can reference artifacts and evidence that support claims about system behavior.

### Dispute Resolution

Because events preserve the historical sequence, disagreements can be analyzed using the recorded trace.

---

# Relationship to State

Boundary Protocol does not eliminate the concept of state.

Instead, **state is treated as an observed artifact**, not the primary record of system history.

States may be captured at specific moments, but the protocol's main structure is the **event chain** that connects actions and observations.

---

# Long-Term Implications

An event-first design allows Boundary Protocol to support:

- distributed verification
- independent auditing
- trace reconstruction
- multi-party dispute analysis

These capabilities are difficult to achieve in purely state-centric systems.

---

# Summary

Boundary Protocol adopts an **event-first architecture** because accountability requires a reliable historical trace.

By recording discrete events and linking them into causal chains, the protocol enables systems, observers, and verifiers to reconstruct and evaluate system behavior across boundaries.