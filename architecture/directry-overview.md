# ANIMA Architecture Documentation

This folder contains detailed architectural documentation for the ANIMA Engine, derived from the Architecture Decision Records (ADRs) and design principles.

---

## Core Architecture Documents

### [ANIMA Architecture Overview](anima-architecture.md)
Comprehensive overview of the entire ANIMA architecture, including all layers and key concepts.

**Topics covered:**
* Engine vs. Identity separation
* Hexagonal Architecture implementation
* Event-based observability and input
* Cognitive Kernel model
* Complete system flow

---

## Detailed Topic Documentation

### [Seed System](seed-system.md)
**Related:** [ADR-001](../adr/ADR-001.md)

Explains how ANIMA separates the engine from identity through declarative Seeds.

**Topics covered:**
* What is a Seed
* Engine vs. Identity
* Architectural invariants
* Use cases (Private instances, ANIMA Prime)

---

### [Module Types and Leases](module-types-and-leases.md)
**Related:** [ADR-003](../adr/ADR-003.md)

Details the lease-based authorization model and three module types.

**Topics covered:**
* gRPC over mTLS communication
* Lease-based authorization
* Lease epochs and anti-desynchronization
* Type I, II, and III modules
* Zero-Lease State
* State persistence rules

---

### [Event Architecture](event-architecture.md)
**Related:** [ADR-004](../adr/ADR-004.md), [ADR-011](../adr/ADR-011.md)

Describes the unified event-based architecture for observability and input processing.

**Topics covered:**
* Event-based observability model
* Execution partitioning
* Event structure and fields
* Canonical event categories (input.nl, input.system, input.semantic)
* Concurrency and multithreading
* Module participation

---

### [Cognitive Kernel](cognitive-kernel.md)
**Related:** [ADR-005](../adr/ADR-005.md), [ADR-008](../adr/ADR-008.md)

Explains how the ANIMA Core operates as a multitasking cognitive kernel.

**Topics covered:**
* Tasks as managed execution units
* Cooperative interruption model
* Interruptions as first-class events
* Execution spans and focus
* Interruptibility policies
* Kernel vs. User space
* Multitasking model

---

### [Domain and Infrastructure](domain-and-infrastructure.md)
**Related:** [ADR-002](../adr/ADR-002.md), [ADR-006](../adr/ADR-006.md), [ADR-007](../adr/ADR-007.md)

Formalizes the Hexagonal Architecture implementation in ANIMA.

**Topics covered:**
* Core principle (domains never talk to the outside world)
* Dependency direction rules
* Approved cross-domain dependency forms
* Cross-domain error translation
* Self-validating schemas
* Infrastructure layer and adapters
* Runtime composition

---

### [AI Model Topology](ai-model-topology.md)
**Related:** [ADR-009](../adr/ADR-009.md)

Details the two-role AI model topology: Cortex and Arcuate.

**Topics covered:**
* Cortex (mandatory cognitive model)
* Arcuate (optional NLP model)
* NLP as a configurable responsibility
* Supported configurations (single model, dual model, Cortex-only)
* Interface-based architecture
* Constraints and invariants

---

### [Adapter-Actuator Split](adapter-actuator-split.md)
**Related:** [ADR-003](../adr/ADR-003.md), [ADR-004](../adr/ADR-004.md), [ADR-005](../adr/ADR-005.md), [ADR-010](../adr/ADR-010.md), [ADR-011](../adr/ADR-011.md)

Specifies how Modules are structured with Adapters and Actuators running out-of-process.

**Topics covered:**
* Core security principle (no third-party code in Core)
* Module package structure
* Adapter responsibilities (pure translation)
* Actuator responsibilities (effectful execution)
* Inbound and outbound flows
* Natural language processing in modules
* Discovery and trust
* Observability
* Security invariants

---

## Key Architectural Principles

1. **Engine ≠ Identity** — Personality comes from Seeds, not the engine
2. **No third-party code in Core** — All modules run out-of-process
3. **Lease-based authorization** — All execution requires valid leases
4. **Domains never import infrastructure** — Clean hexagonal boundaries
5. **Event-based everything** — No traditional logs, only structured events
6. **Instance-scoped memory** — No cross-instance sharing
7. **Cortex mandatory, Arcuate optional** — Cognition vs. language separation
8. **Core supervises, doesn't execute** — Cognitive kernel model
9. **Cooperative interruption** — All actions are interruptible by design
10. **Events are the source of truth** — Immutable, auditable facts

---

## Architecture Decision Records (ADRs)

For the formal decision records that define this architecture, see the [ADR folder](../adr/):

* [ADR-001](../adr/ADR-001.md) — Separation of ANIMA Engine and Identity via Seed System
* [ADR-002](../adr/ADR-002.md) — Self-Validating Schemas with Internal-Only Constraints
* [ADR-003](../adr/ADR-003.md) — Core ↔ Module Communication Protocol, Module Types, and Lease Lifecycle
* [ADR-004](../adr/ADR-004.md) — Observability, Event Logging, and Execution Traceability
* [ADR-005](../adr/ADR-005.md) — Interruption & Preemption Model
* [ADR-006](../adr/ADR-006.md) — Domain Dependency & Sharing Rules
* [ADR-007](../adr/ADR-007.md) — Infrastructure Layer & Adapter Rules
* [ADR-008](../adr/ADR-008.md) — ANIMA Core Behaves as a Cognitive Kernel
* [ADR-009](../adr/ADR-009.md) — Configurable AI Model Topology with Mandatory Cortex and Optional Arcuate
* [ADR-010](../adr/ADR-010.md) — Adapter–Actuator Split with Strict Process Boundary
* [ADR-011](../adr/ADR-011.md) — Event-Based Input Architecture

---

## Quick Reference

### System Flow

```
User/Environment
  ↓ (capture)
Module
  ↓ (translate to events)
Adapter
  ↓ (input.nl / input.system / input.semantic)
Security
  ↓
Cognitive Kernel (Cortex + optional Arcuate)
  ↔ Memory / Seed
  ↓ (intent + capability URN)
Security (validate)
  ↓
Adapter (validate schema)
  ↓
Actuator (execute)
  ↓
Effect + Events
```

### Module Types

* **Type I** — Ephemeral Private (lease-bound, terminates on Zero-Lease)
* **Type II** — Resident Private (standby on Zero-Lease, single Core)
* **Type III** — Resident Shared (multi-tenant, infrastructure-governed)

### Event Types

* **input.nl** — Natural language (pre-semantic)
* **input.system** — System/mechanical facts
* **input.semantic** — Post-interpretation asserted meaning
* **capability.invoked** — Capability execution started
* **capability.completed** — Capability execution finished
* **capability.interrupted** — Capability execution interrupted

---

## Navigation

* [Main README](../README.md)
* [ADR Index](../adr/)
* [Vision](../vision.md)
* [Foundations](../foundations.md)
* [Memory Integrity](../memory-integrity.md)
* [Safety Model](../safety-model.md)
* [Threat Model](../threat-model.md)
