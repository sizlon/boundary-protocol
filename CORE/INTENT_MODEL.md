# Intent Model

## 1. Purpose

The Intent model defines how desired outcomes are declared within the Boundary Protocol.

An **Intent** represents a declaration that a particular condition or action should occur. It does not perform execution and does not guarantee that execution will happen. It only expresses the desired condition that systems or actors may attempt to realize.

Intent is the starting point of all verifiable action histories within the protocol.

---

## 2. Core Definition

An **Intent** is an immutable declaration of a desired outcome associated with a subject within a system boundary.

Conceptually:

```
Intent = declaration of desired state
```

An intent answers the question:

> What should happen?

Examples:

- Deploy service version `v2`
- Rotate credentials
- Enable TLS enforcement
- Update configuration

---

## 3. Intent Characteristics

### Immutability

Once created, an intent cannot be modified.

Changes must be expressed through new intents that reference previous ones.

### Content Addressing

Each intent is identified by a deterministic identifier derived from its contents.

```
intent_id = hash(intent_document)
```

This allows intents to be verified independently.

### Declarative Nature

Intent does not define execution steps.

It only declares the desired outcome.

Execution details belong to transitions and implementations.

---

## 4. Intent Structure

A minimal conceptual structure for an intent includes:

```
intent
├─ intent_id
├─ subject
├─ desired_state
├─ declared_by
├─ declared_at
└─ relations
```

### subject

Identifies the system resource the intent concerns.

Example:

```
subject:
  system: deployment
  resource: serviceA
```

### desired_state

Describes the expected resulting condition.

Example:

```
desired_state:
  version: v2
```

### declared_by

Identity of the actor declaring the intent.

Actors may include:

- humans
- automated systems
- services
- agents

### declared_at

Timestamp representing when the intent was declared.

### relations

Optional links to other intents.

---

## 5. Intent Relations

Intent objects may reference other intents to represent evolution and relationships.

Supported conceptual relationships include:

### revision_of

Represents a direct update of an earlier intent.

```
intent_v2
revision_of: intent_v1
```

### branch_of

Represents an alternative intent derived from another intent.

```
intent_A
branch_of: intent_root
```

### supersedes

Indicates that an intent replaces another intent.

```
intent_new
supersedes: intent_old
```

### derived_from

Indicates that an intent conceptually derives from another intent but is not a direct revision.

---

## 6. Intent Lifecycle

The protocol itself does not define complex workflow states.

However, intents may conceptually exist in minimal semantic states:

- active
- satisfied
- failed
- abandoned

These states are derived from transition histories rather than stored as mutable properties.

---

## 7. Intent and Transition Relationship

An intent does not execute actions itself.

Instead, transitions attempt to realize the intent.

```
Intent
   ↓
Transition Attempt
   ↓
Observed State
```

Multiple transitions may reference the same intent.

Example:

```
intent: deploy_v2

transition_attempt_1 → fail
transition_attempt_2 → fail
transition_attempt_3 → success
```

---

## 8. Deterministic Identity

Because intents are immutable and content-addressed, they can be shared and verified across systems.

Two systems receiving the same intent document should produce the same `intent_id`.

This property allows intent registries and distributed verification.

---

## 9. Scope of Intent

Intent describes **desired outcomes**, not execution strategies.

The protocol intentionally avoids specifying:

- execution methods
- orchestration strategies
- runtime environments

Those concerns belong to implementations.

---

## 10. Summary

The Intent model provides a minimal structure for declaring desired outcomes.

Key properties:

- immutable
- content-addressed
- declarative
- verifiable

Intent forms the first element in the protocol's causal chain:

```
Intent
   ↓
Transition
   ↓
State
```

This structure allows actions across system boundaries to be recorded and verified.

