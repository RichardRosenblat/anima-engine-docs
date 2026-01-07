# ANIMA Foundations

This document provides navigation to the three foundational documents that define ANIMA's constitutional law, canonical vocabulary, and architectural boundaries.

---

## Foundation Documents

### [1️⃣ Project Charter](project-charter.md)

**Purpose:** Defines *"what exists"* and *"what is forbidden"*.

**Contents:**
* What is ANIMA (and what it is not)
* Core purpose and values
* Explicit non-goals
* Architectural invariants
* Success criteria
* Governance model

**Key Principle:** Every feature must be justifiable against this charter.

**Read:** [Project Charter →](project-charter.md)

---

### [2️⃣ Canonical Glossary](canonical-glossary.md)

**Purpose:** Establishes the canonical vocabulary of ANIMA.

**Contents:**
* System components (Engine, Core, Seed, Memory, etc.)
* Architectural concepts (Module, Adapter, Actuator, Capability, etc.)
* AI models (Cortex, Arcuate)
* Observability concepts (Event, Execution, Lease, etc.)
* Process and state (Zero-Lease State, Task Recovery Grace Period)

**Key Principle:** These definitions must **never drift**.

**Read:** [Canonical Glossary →](canonical-glossary.md)

---

### [3️⃣ System Boundaries](system-boundaries.md)

**Purpose:** Defines what ANIMA can and cannot do by design.

**Contents:**
* What the engine can NEVER do (10 architectural invariants)
* What MUST ALWAYS require confirmation (5 categories)
* What is delegated to modules (5 responsibilities)
* Security boundaries
* Failure modes and boundaries

**Key Principle:** Boundaries are enforced by architecture, not convention. Violations are bugs.

**Read:** [System Boundaries →](system-boundaries.md)

---

## Relationship to Architecture

These foundation documents work together with the [Architecture Documentation](architecture/README.md) to define ANIMA completely:

* **Foundations** (this) → Constitutional principles and vocabulary
* **Architecture** → Detailed implementation and design patterns
* **ADRs** → Formal decision records explaining why

---

## Quick Reference

### Core Values

1. **Truth over confidence** — Admit uncertainty rather than fabricate
2. **Intent over execution** — Produce intent, not direct effects
3. **Modularity over monolith** — Interchangeable components
4. **Safety over capability** — Permissions first, features second
5. **Configurability over hardcoding** — Data-driven behavior (Seeds)
6. **Isolation over convenience** — Security through separation

### Architectural Invariants

1. Engine is identity-agnostic (personality from Seeds)
2. Core never loads third-party code (modules out-of-process)
3. All execution requires valid leases
4. Domains never import infrastructure (hexagonal architecture)
5. All observability is event-based (no traditional logs)
6. Memory is strictly instance-scoped
7. Cortex is mandatory, Arcuate is optional
8. Core supervises, doesn't execute (cognitive kernel)
9. All actions are interruptible (cooperative interruption)
10. Events are the source of truth (immutable, auditable)

### Key Boundaries

* **Core never touches the world** — Intent, not effects
* **No lease, no execution** — Zero-Lease State
* **No cross-instance memory** — Instance-local only
* **No self-modification** — Stable code and Seeds
* **Dangerous actions require confirmation** — Explicit user approval

---

## Summary

> **ANIMA is an engine for growing identities, not a single personality.**
>
> **These foundations are constitutional. They define what ANIMA is and can never become.**
>
> **Precision in language prevents confusion in architecture.**
