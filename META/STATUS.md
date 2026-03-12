# STATUS

This document records the current status of the Boundary Protocol specification.

Its purpose is to make the maturity, intended audience, and current expectations of the protocol explicit.

---

# Current Status

**Status:** Experimental  
**Version:** v0.1  
**Stability:** Not production-stable  
**Audience:** Early implementers, reviewers, and protocol designers

Boundary Protocol is currently in the **early experimental design stage**.

At this stage:

- the conceptual model is being stabilized
- core protocol primitives are being refined
- artifact contracts are being defined
- validation rules are being established
- schemas are being introduced

Implementers should assume that **breaking changes are still possible**.

---

# What Is Considered Stable

The following areas are currently treated as the most stable parts of the protocol:

- the protocol's accountability positioning
- the core primitives: **intent**, **transition**, and **state**
- the responsibility model: **identity**, **authority**, and **watcher**
- the event-first design direction

These areas may still evolve, but they represent the emerging center of the specification.

---

# What Is Still Evolving

The following areas should still be considered under active design:

- schema details
- artifact field naming conventions
- event envelope details
- validation and trace reconstruction strategy
- implementation guidance for SDKs and verifiers

These may change as the protocol moves toward structural stabilization.

---

# Intended Use at This Stage

At the current stage, Boundary Protocol is intended for:

- conceptual review
- protocol design discussion
- early prototype implementations
- experimentation in AI accountability and autonomous system tracing

It is **not yet intended** as a frozen production dependency.

---

# Near-Term Milestones

The next expected milestones are:

1. stabilization of the protocol skeleton
2. completion of the initial schema set
3. example trace publication
4. early implementation experiments
5. transition toward a later `v0.x` stabilization phase

---

# Forward Path

The anticipated maturity path is:

```text
v0.1  early experimental design
v0.5  structural stabilization
v0.9  freeze candidate
v1.0  stable protocol
```

This path is aspirational rather than guaranteed. Advancement depends on successful implementation and review.

---

# Summary

Boundary Protocol is currently an **experimental accountability protocol specification**.

Its conceptual structure is now visible and increasingly coherent, but the protocol should still be treated as under active development until later stabilization stages are reached.