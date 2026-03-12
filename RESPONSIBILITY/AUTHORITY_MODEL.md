# Authority Model

## 1. Purpose

The Authority model defines how permission and decision power are represented within the Boundary Protocol.

While the Identity model defines *who exists*, the Authority model defines *who is allowed to perform specific actions* within a protocol context.

Authority determines which identities are permitted to:

- declare intents
- execute transitions
- publish observations

The protocol defines only the minimal structure required to express authority relationships.

---

## 2. Core Definition

An **Authority** represents the permission granted to an identity to perform specific protocol actions.

Conceptually:

```
Authority = permission granted to an identity
```

Authority links **identities** to **capabilities** within a defined scope.

---

## 3. Authority Scope

Authority is always contextual. It exists within a specific **scope**.

Examples of scopes may include:

- a specific resource
- a system boundary
- a namespace
- an intent group

Example:

```
authority
├─ identity: deployment_service
├─ capability: transition_execution
└─ scope: serviceA
```

This means the identity is authorized to execute transitions for that scope.

---

## 4. Authority Capabilities

Authorities grant capabilities that allow identities to perform protocol actions.

Common capability types include:

- **intent_declaration** — declare intents
- **transition_execution** — perform transitions
- **state_observation** — publish observable states

Additional capability types may be defined by implementations.

---

## 5. Authority Structure

A minimal conceptual structure for an authority includes:

```
authority
├─ authority_id
├─ identity
├─ capability
├─ scope
├─ granted_by
└─ granted_at
```

### authority_id

Deterministic identifier of the authority record.

### identity

Identity receiving the authority.

### capability

Action type the identity is permitted to perform.

### scope

Defines the boundary within which the authority applies.

### granted_by

Identity that granted the authority.

### granted_at

Timestamp of when the authority was granted.

---

## 6. Authority Delegation

Authorities may be delegated.

Example:

```
admin_identity
   ↓ grants
service_identity
   ↓ executes
transition
```

Delegation allows distributed systems to operate without centralized control.

Implementations may enforce policies limiting delegation.

---

## 7. Authority and Protocol Actions

Authority applies to protocol operations.

Examples:

```
Intent
  declared_by → identity
  requires → intent_declaration authority

Transition
  actor → identity
  requires → transition_execution authority

State
  observed_by → identity
  requires → state_observation authority
```

Authority therefore controls **who may produce protocol artifacts**.

---

## 8. Authority and Governance

The protocol does not enforce governance policies.

Instead, it provides the minimal structure required for systems to evaluate authority relationships.

Higher-level systems may implement:

- role-based policies
- approval workflows
- multi-party authorization

These mechanisms exist outside the protocol core.

---

## 9. Authority Evolution

Authority assignments may change over time.

New authority records may:

- grant authority
- modify authority
- revoke authority

Because protocol artifacts are immutable, authority history remains auditable.

---

## 10. Summary

The Authority model defines how permissions are expressed within the protocol.

Key properties:

- identity-linked
- scope-bound
- capability-based

Authority enables systems to determine **who is permitted to declare, execute, or observe protocol actions**.

