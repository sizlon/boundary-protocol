# VERSION

This document defines the versioning model for the Boundary Protocol specification.

Protocol versioning is intended to ensure stability for implementers while allowing the protocol to evolve over time.

The version number reflects the maturity and compatibility expectations of the protocol specification.

---

# Version Format

Boundary Protocol follows a simplified semantic versioning model:

```
MAJOR.MINOR
```

Examples:

```
v0.1
v0.5
v1.0
v2.0
```

Patch-level versions are intentionally avoided in the specification repository. Minor corrections should not change protocol meaning.

---

# Version Stages

## v0.x — Experimental Design

Versions beginning with `0.x` represent an experimental phase.

Characteristics:

- The protocol model is still evolving
- Core concepts may change
- Schemas may change
- Implementations should expect breaking changes

The purpose of this stage is to stabilize the **conceptual model**.

---

## v0.5 — Structural Stabilization

This stage indicates that the overall structure of the protocol is mostly defined.

Characteristics:

- Core model is stable
- Responsibility model is stable
- Contract artifacts are mostly defined
- Validation logic is mostly defined

However, schemas and event structures may still evolve.

---

## v0.9 — Freeze Candidate

This stage signals that the protocol is approaching stability.

Characteristics:

- Protocol primitives are frozen
- Artifact types are frozen
- Schema changes should be minimal

The goal of this phase is to validate the protocol through early implementations.

---

## v1.0 — Stable Protocol

Version `v1.0` represents the first stable release of Boundary Protocol.

Characteristics:

- Core protocol primitives are fixed
- Artifact contracts are stable
- Schema compatibility is guaranteed
- Implementations may safely depend on the specification

Breaking changes after this point require a new **major version**.

---

# Breaking Changes

A change is considered **breaking** if it:

- modifies the meaning of protocol primitives
- alters artifact semantics
- invalidates previously valid traces

Breaking changes require incrementing the **major version**.

Example:

```
v1.0 → v2.0
```

---

# Non-Breaking Changes

Non-breaking changes may include:

- clarification of documentation
- additional examples
- expanded rationale

These changes do not affect the protocol version.

---

# Compatibility Principle

Once the protocol reaches **v1.0**, the following principle applies:

> A valid trace produced under version `v1.x` must remain interpretable under future versions of the protocol.

This ensures long-term durability of accountability traces.

---

# Current Status

The current version of Boundary Protocol is:

```
v0.1
```

This indicates that the protocol is in the **early experimental design stage**.

Core concepts are being stabilized before wider implementation and adoption.

