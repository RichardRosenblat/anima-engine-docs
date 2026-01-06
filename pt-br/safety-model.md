# SAFETY_MODEL.md

## Purpose

This document describes **how ANIMA enforces safety mechanically**.

It does not define moral values, ethical theories, or behavioral ideals.
Safety in ANIMA is achieved through **explicit constraints, verification steps, and execution boundaries** enforced by the engine.

---

## Safety as an Engine Property

Safety in ANIMA is **not prompt-based** and **not personality-based**.

It is enforced by:

* Capability gating
* Permission checks
* Confirmation flows
* Epistemic validation
* Explicit uncertainty handling
* Execution isolation

No component is trusted by default — including adapters, inputs, or the language model.

---

## Epistemic Validation

ANIMA tracks the provenance of all knowledge used in decision-making.

### Knowledge States

All knowledge is classified as one of a set of states (may evolve over time):

* **Observed:** Directly perceived or input data.
* **Remembered:** Retrieved from memory layers with confidence metadata.
* **Inferred:** Derived through reasoning with uncertainty levels.

All knowledge is tagged with its source and confidence level.

### Understanding meaning and provenance without blind trust

ANIMA prevents total blind trust (e.g. utilizing other language models as enforcers) through natural language processing techniques and non-compliance detection.

Through these mechanisms, ANIMA can identify:
* Contradictions
* Inconsistencies
* Ambiguities
* When knowledge confidence is insufficient for a decision

## Capability Gating

ANIMA cannot execute arbitrary actions.

All actions must map to a **declared capability module**.

### Capability Rules

* Capabilities are:

  * Explicit
  * Discoverable
  * Auditable

* Capabilities must be:

  * Enabled by the engine
  * Allowed by the active seed
  * Permitted for the current user role

### Important Constraint

> **Seeds may restrict capabilities, but never grant new execution power.**

This ensures:

* No identity can exceed the engine’s hard limits
* Licensing and safety boundaries remain enforceable
* Capabilities cannot be smuggled through personality or memory

---

## Confirmation Flows

Some capabilities require **human confirmation** before execution.

### When Confirmation Is Required

* Irreversible actions
* Actions affecting external systems
* High-cost or high-risk operations
* Actions with ambiguous intent

### Confirmation Properties

* Explicit (not inferred)
* Time-bound
* Multi-factor
* Logged
* Tied to a specific action instance

Adapters **cannot self-assert consent**.
Confirmation must be validated by the engine through one or more trusted channels.

---

## No Direct Execution from Natural Language

Natural language **never triggers execution directly**.

The execution pipeline is strictly:

```
Input → Interpretation → Intent → Capability Proposal → Validation → Confirmation → Execution
```

This prevents:

* Prompt injection
* Linguistic ambiguity causing actions
* Adapters escalating intent
* “Just do it” failure modes

If intent cannot be confidently mapped, **execution does not occur**.

---

## Hallucinations Are Treated as Failures

ANIMA treats hallucinations as **system errors**, not stylistic quirks.

### What Counts as a Hallucination

* Stating facts without sufficient evidence
* Claiming capabilities that are unavailable
* Inventing memories or permissions
* Guessing user intent

### Enforcement

* The engine tracks knowledge provenance:

  * observed
  * remembered
  * inferred

* If confidence is insufficient:

  * ANIMA must say so explicitly
  * The action is blocked or downgraded

> A confident lie is considered worse than a refused answer.

---

## Uncertainty Handling

Uncertainty is **explicitly represented**, not hidden.

### Mechanisms

* Confidence thresholds for intent classification
* Required clarification when ambiguity is high
* Explicit “I don’t know” states
* Refusal when execution risk exceeds certainty

Uncertainty is not treated as failure —
**acting without acknowledging uncertainty is.**

---

## Memory Safety

Memory is constrained to prevent unsafe inference and drift.

* No cross-instance memory
* No implicit memory promotion
* Narrative memory requires explicit curation
* Seeds are immutable after initialization

This prevents:

* Identity corruption
* Silent personality drift
* Accidental belief formation

---

## Auditability

Every safety-relevant decision is logged:

* Capability checks
* Permission denials
* Confirmation requests
* Execution attempts
* Refusals

Logs are designed to answer:

> “Why did ANIMA act — or refuse — in this case?”

---

## What the Safety Model Does *Not* Do

This model does **not** attempt to:

* Enforce moral alignment
* Judge user intent beyond execution risk
* Prevent misuse on compromised hosts
* Act as a sandbox or secure enclave
* Replace human responsibility

Those concerns are explicitly out of scope.

---

## Summary

ANIMA’s safety model is based on:

* Explicit capability boundaries
* Engine-level enforcement
* Human confirmation for risk
* Transparent uncertainty
* Treating hallucinations as errors

Safety is not emergent behavior.
It is **designed, enforced, and auditable**.
