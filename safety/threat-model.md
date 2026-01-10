# ANIMA — Threat Model

**Related Documentation:**
* [Canonical Glossary](../foundations/canonical-glossary.md) - Definitions of User Risk Level and System Risk Level
* [Safety Model](safety-model.md) - Safety enforcement mechanisms
* [System Boundaries](../foundations/system-boundaries.md) - Architectural constraints

---

## Purpose

This document outlines the **explicit threat model** for ANIMA.

It defines:

* What ANIMA is designed to defend against
* What ANIMA explicitly does *not* attempt to defend against
* The assumptions made about the host environment

This is not a claim of perfect security.
It is a statement of **intentional boundaries and responsibility**.

---

## System Overview (Security Context)

ANIMA is a **private, modular AI engine** running within a user-controlled environment.

Key characteristics:

* Instance-local memory
* Capability-gated execution
* No internet access by default
* No shared global state
* Explicit human confirmation for actions with high User Risk Level (see [Canonical Glossary](../foundations/canonical-glossary.md))

ANIMA assumes **no implicit trust** in inputs, adapters, or capabilities.

---

## Assets to Protect

ANIMA prioritizes protection of the following assets:

1. **Instance Memory**

   * User conversations
   * Learned preferences
   * Narrative identity continuity

2. **Seed Integrity**

   * Identity constraints
   * Capability policies
   * Behavioral boundaries

3. **Capability Boundaries**

   * Prevention of unauthorized execution
   * Enforcement of permission checks

4. **User Intent**

   * Accurate representation of user requests
   * Protection against unintended actions

5. **Auditability**

   * Clear reasoning paths
   * Action traceability

---

## Threats ANIMA Defends Against

### 1. Cross-Instance Data Leakage

**Threat:**
Memory or identity information leaking between ANIMA instances.

**Mitigation:**

* Strict instance isolation
* No shared memory layers
* No global learning or training pool
* Explicit prohibition of cross-instance reads

---

### 2. Capability Escalation

**Threat:**
A user, adapter, or module attempting to execute actions beyond authorized capabilities.

**Mitigation:**

* Centralized capability registry
* Mandatory permission checks
* Seed-level capability restrictions
* Human confirmation for actions with high User Risk Level (see [Canonical Glossary](../foundations/canonical-glossary.md))

---

### 3. Prompt Injection & Input Manipulation

**Threat:**
Malicious input attempting to override behavior, policies, or safety constraints.

**Mitigation:**

* Separation of reasoning and execution
* Non-textual enforcement of safety rules
* Seed constraints enforced outside prompts
* No direct execution from natural language

---

### 4. Hallucinated Authority or Knowledge

**Threat:**
ANIMA presenting guesses or fabrications as facts, especially in contexts with high System Risk Level or User Risk Level (see [Canonical Glossary](../foundations/canonical-glossary.md)).

**Mitigation:**

* Explicit uncertainty tracking
* Knowledge provenance labels (observed / inferred / unknown)
* Refusal to act on uncertain information when safety is impacted

---

### 5. Silent Behavior Drift

**Threat:**
Identity changes occurring without visibility or explanation.

**Mitigation:**

* Read-only seeds after initialization
* Memory promotion rules
* Narrative memory curation
* Inspectable state changes

---

### 6. Unauthorized Observation or Monitoring

**Threat:**
ANIMA observing or recording users or environments without consent.

**Mitigation:**

* No passive sensing
* Adapter-level permission requirements
* Explicit user initiation for observation
* Clear visibility into active inputs

---

## Threats Explicitly Out of Scope

ANIMA does **not** attempt to defend against the following:

### 1. Host System Compromise

If the host operating system, container, or hardware is compromised, ANIMA assumes:

* Memory confidentiality cannot be guaranteed
* Capability boundaries may be bypassed

ANIMA is not a sandbox or secure enclave.

---

### 2. Malicious Engine Modification

If the ANIMA engine source or binaries are modified:

* All guarantees are void
* Safety mechanisms may be disabled

Code integrity is the responsibility of the distributor and host.

---

### 3. Side-Channel Attacks

ANIMA does not protect against:

* Timing attacks
* Power analysis
* Hardware side-channels

These are outside the intended threat surface.

---

### 4. Adversarial Model Extraction

ANIMA does not attempt to prevent:

* Model inversion
* Weight extraction
* Statistical probing of underlying models

The engine treats the underlying model as a replaceable component.

---

### 5. Human Social Engineering

ANIMA cannot fully defend against:

* Users convincing other humans to misuse the system
* Trust placed outside the system’s guarantees

Human behavior remains a human responsibility.

---

## Assumptions About the Host Environment

ANIMA assumes:

* The host OS enforces process isolation
* File system permissions are respected
* Network access is controlled externally
* Secrets (keys, licenses) are stored securely
* Adapters are not malicious by default

Violating these assumptions invalidates parts of this threat model.

---

## Design Philosophy

ANIMA favors:

* **Explicit denial over silent failure**
* **Auditability over opacity**
* **Bounded capability over maximal power**

Security is achieved through **architectural constraints**, not through trust in intelligence or intent.

---

## Final Statement

This threat model is intentionally conservative.

ANIMA does not attempt to solve all security problems.
She attempts to solve **the right ones**, clearly and honestly.

By documenting what is in scope and what is not, ANIMA remains a system that users can reason about, trust, and grow with.
