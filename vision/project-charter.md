# Project Charter

**Related Documentation:**
* [Vision](vision.md) - Long-term vision and goals
* [Non-Goals](non-goals.md) - Explicit non-goals and boundaries
* [Architecture Overview](architecture/anima-architecture.md) - Complete system architecture

---

## What is ANIMA?

ANIMA is a **private, modular AI engine** designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

ANIMA is:

* A **private AI runtime** for evolving identities
* An **identity-agnostic engine** (personality comes from Seeds)
* A **cognitive kernel** that supervises behavior
* An **event-reasoning system** that operates on structured facts
* A **hexagonal architecture** with clean domain boundaries
* A **lease-based module system** over gRPC with mTLS

ANIMA is **not**:

* A chatbot or conversation wrapper
* An autonomous agent that self-expands
* A single personality or hardcoded character
* An internet-crawling system by default
* A shared-memory or cloud-synced system
* A replacement for human judgment

---

## Core Purpose

ANIMA exists to:

1. **Provide a private, evolving AI runtime**
   * Host long-lived artificial identities
   * Enable instance-local memory and learning
   * Support continuous operation and task management

2. **Separate engine from identity**
   * Engine: reasoning, planning, memory mechanics, security
   * Identity: personality, tone, behavioral policies (defined by Seed)
   * Enable multiple independent instances from one engine

3. **Support multiple incarnations via Seeds**
   * Private user instances with custom Seeds
   * Protected identities (e.g., ANIMA Prime for streaming)
   * No cross-instance memory or state sharing

4. **Enable safe, auditable interaction with the world**
   * Lease-based authorization for all capabilities
   * Event-based observability (structured, traceable facts)
   * Cryptographic security boundaries
   * Human confirmation for dangerous actions
   * Full audit trails for all decisions and effects

---

## Core Values

These values are **constitutional**. Every feature must be justifiable against them.

### Truth over Confidence

ANIMA prioritizes accuracy and honesty over appearing certain.

* Track whether information is observed, remembered, inferred, or unknown
* Admit uncertainty rather than fabricate
* Confident hallucination is treated as a bug
* Memory is fallible and queryable, not blindly trusted

### Intent over Execution

ANIMA focuses on understanding and fulfilling intentions rather than executing commands blindly.

* Core produces **intent**, not direct effects
* Intent includes: what, when, where, how much, why, contingencies, confidence
* Execution is delegated to supervised modules
* Intent is auditable, loggable, and replayable

### Modularity over Monolith

ANIMA is designed with interchangeable components for flexibility and long-term evolution.

* Clear separation of concerns (domains, infrastructure, modules)
* Ports and adapters (hexagonal architecture)
* Modules run out-of-process from Core
* No third-party code in Core
* Hot-swappable implementations via interfaces

### Safety over Capability

ANIMA prioritizes user safety and ethical considerations above expanding functionality.

* Permissions enforced at engine level
* Capability allow/deny lists
* Human confirmation for dangerous actions
* Lease-based authorization (no lease, no execution)
* Fail closed on uncertainty
* Explicit, never implicit, permissions

### Configurability over Hardcoding

ANIMA emphasizes external configuration rather than fixed behaviors.

* Personality defined by data (Seeds), not code
* Behavioral policies configurable per instance
* Capability restrictions defined declaratively
* Memory policies seed-configurable
* No hardcoded personality or tone

### Isolation over Convenience

ANIMA values keeping components and processes separate to enhance security and reliability.

* Instance-scoped memory (no cross-instance sharing)
* Process boundaries between Core and Modules
* Execution partitioning for observability
* Cryptographic lease boundaries
* Domain isolation (hexagonal architecture)

---

## Non-Goals (Explicit)

These are **explicitly excluded** from ANIMA's purpose:

### Technical Non-Goals

* **No self-modifying code**
  * Engine code remains stable
  * Behavior evolves through memory and Seeds, not code mutation
  * No dynamic code generation or plugin loading into Core

* **No uncontrolled autonomy**
  * All long-lived tasks are user-initiated
  * Tasks are interruptible and auditable
  * No self-expanding goals or capabilities
  * Constrained by permissions and Seeds

* **No internet access unless granted via capability**
  * Network access is opt-in, never default
  * Requires explicit capability and permission
  * Subject to lease enforcement and audit

* **No shared memory between instances**
  * Each instance has isolated, instance-local memory
  * No cross-user learning or global memory
  * No data sharing beyond model training
  * Prevents identity and privacy leakage

* **No implicit user permissions**
  * All permissions must be explicit
  * Capabilities declared and gated
  * Dangerous actions require human confirmation
  * No silent privilege escalation

### Architectural Non-Goals

* **Not a request-response chatbot**
  * Long-lived, stateful cognitive system
  * Concurrent task execution
  * Interrupt-driven, not blocking

* **Not a monolithic system**
  * Modular, ports-and-adapters architecture
  * Clean domain boundaries
  * Replaceable infrastructure

* **Not a cloud service by default**
  * Private, local-first runtime
  * User owns instance data
  * No telemetry or data upload without explicit capability

---

## Architectural Invariants

These constraints are **non-negotiable** and enforced by design:

1. **Engine is identity-agnostic** — personality comes from Seeds, not code
2. **Core never loads third-party code** — modules run out-of-process over gRPC+mTLS
3. **All execution requires valid leases** — no lease, no execution (Zero-Lease State)
4. **Domains never import infrastructure** — only ports/interfaces (hexagonal architecture)
5. **All observability is event-based** — no traditional logs, only structured facts
6. **Memory is strictly instance-scoped** — no cross-instance sharing ever
7. **Cortex is mandatory, Arcuate is optional** — cognition essential, language is a capability
8. **Core supervises, doesn't execute** — cognitive kernel model
9. **All actions are interruptible** — cooperative interruption by design
10. **Events are the source of truth** — immutable, auditable, execution-scoped facts

---

## Success Criteria

ANIMA is successful when it enables:

* **Long-lived identities** that evolve consistently over time
* **Safe interaction** with the world through auditable capabilities
* **Private instances** where users own their data and memory
* **Multiple incarnations** from a single engine via Seeds
* **Trust and transparency** through observable, explainable behavior
* **Architectural stability** that supports long-term evolution

---

## Governance

This charter is **constitutional law** for ANIMA.

* All architectural decisions must align with these values
* All features must be justifiable against this charter
* All non-goals must be actively prevented by design
* All invariants must be enforced by architecture, not convention

Violations of this charter are architectural bugs, not feature requests.

---

## Summary

> **ANIMA is an engine for growing identities, not a single personality.**
>
> **ANIMA does not execute instructions. It supervises behavior.**
>
> **ANIMA does not log text. It records facts.**
>
> **ANIMA prioritizes consistency, honesty, and safety over capability.**
