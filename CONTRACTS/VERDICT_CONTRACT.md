# Verdict Contract

## 1. Purpose

The Verdict Contract defines how the outcome of verification is represented within the Boundary Protocol.

While transition events record execution attempts and attestations provide observable evidence, a **verdict** represents the result of evaluating whether an intent has been successfully realized.

The verdict is therefore the **evaluation layer** of the protocol.

---

## 2. Core Definition

A **Verdict** is a structured evaluation stating whether a transition satisfied the intent based on available evidence.

Conceptually:

```
Verdict = evaluation(intent, transition, attestations)
```

A verdict answers the question:

> Did the transition satisfy the intent?

---

## 3. Contract Goals

The Verdict Contract ensures that verification outcomes are:

- explicit
- reproducible
- evidence-referenced
- attributable

The contract does not mandate a specific verification algorithm. It only defines how the **result of verification** is expressed.

---

## 4. Minimal Verdict Structure

A minimal verdict contains the following fields:

```
verdict
├─ verdict_id
├─ intent_ref
├─ transition_ref
├─ evaluated_by
├─ evaluated_at
├─ result
└─ evidence_refs
```

---

## 5. Field Definitions

### verdict_id

Deterministic identifier of the verdict record.

```
verdict_id = hash(verdict_document)
```

This enables independent verification of verdict artifacts.

---

### intent_ref

Reference to the intent being evaluated.

```
intent_ref: <intent_id>
```

---

### transition_ref

Reference to the transition event that attempted to realize the intent.

```
transition_ref: <event_id>
```

---

### evaluated_by

Identity responsible for producing the verdict.

This may include:

- verification engines
- automated systems
- governance services
- human auditors

---

### evaluated_at

Timestamp indicating when the evaluation occurred.

Example:

```
evaluated_at: 2026-03-07T12:15:00Z
```

---

### result

The evaluation outcome.

Common values include:

- PASS
- FAIL
- UNKNOWN

Additional result types may be defined by implementations.

---

### evidence_refs

References to attestations or state observations used during evaluation.

Example:

```
evidence_refs:
  - attestation_a1
  - attestation_b3
```

These references allow verification decisions to be independently reviewed.

---

## 6. Example Verdict

Example verdict record:

```
verdict
  verdict_id: v3c82f...
  intent_ref: intent_7f9a
  transition_ref: e2b1f8
  evaluated_by: verifier_engine_A
  evaluated_at: 2026-03-07T12:15:00Z
  result: PASS
  evidence_refs:
    - attestation_a1
    - attestation_b3
```

---

## 7. Multiple Verdicts

Multiple evaluators may produce verdicts for the same transition.

Example:

```
verifier_A → verdict_1
verifier_B → verdict_2
verifier_C → verdict_3
```

Higher-level systems may use these verdicts to implement:

- consensus
- quorum-based validation
- dispute resolution

---

## 8. Verdict Immutability

Verdicts are immutable artifacts.

Once recorded, a verdict must not be modified.

Updated evaluations must be expressed as new verdict records.

This ensures a transparent audit trail of verification decisions.

---

## 9. Verification Flow

Within the protocol lifecycle:

```
Intent
   ↓
Transition Event
   ↓
State Observation
   ↓
Attestation
   ↓
Verdict
```

The verdict represents the final evaluation stage.

---

## 10. Summary

The Verdict Contract defines how verification outcomes are expressed.

Key properties:

- evaluation-based
- evidence-referenced
- evaluator-attributed
- immutable

Verdicts provide the formal statement of whether an intent has been successfully realized within the Boundary Protocol.

