# 🧩 ANIMA Architecture — Comprehensive Overview

**Related Documentation:**
* [Seed System](seed-system.md) — Identity initialization and separation
* [Module Types and Leases](module-types-and-leases.md) — Module lifecycle and authorization
* [Event Architecture](event-architecture.md) — Observability and input system
* [Cognitive Kernel](cognitive-kernel.md) — Core as a multitasking supervisor
* [Domain and Infrastructure](domain-and-infrastructure.md) — Hexagonal architecture implementation
* [AI Model Topology](ai-model-topology.md) — Cortex and Arcuate
* [Adapter-Actuator Split](adapter-actuator-split.md) — Module structure

**Related ADRs:** All ADRs (ADR-001 through ADR-011)

---

## Introduction

ANIMA is a **private, modular AI engine** designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

The architecture follows **Hexagonal Architecture** and **Domain-Driven Design (DDD)** principles, with a strong emphasis on:

* Separation of concerns
* Security by design
* Observable, auditable behavior
* Identity isolation
* Modular extensibility

---

## Core Architectural Principles

### 1. Engine ≠ Identity

The ANIMA Engine is identity-agnostic. Personality and behavior are introduced through **Seeds** (see [Seed System](seed-system.md)).

* Engine: reasoning, planning, MTL (Medial Temporal Lobe, memory handling), security
* Identity: personality, tone, behavioral policies (defined by Seed)

### 2. Hexagonal Architecture

> **Domains must never talk directly to the outside world.**

* Domains define **ports** (interfaces)
* Infrastructure provides **adapters** (implementations)
* Runtime composes the system

See [Domain and Infrastructure](domain-and-infrastructure.md) for details.

### 3. Event-Based Observability and Input

* All observability is expressed as **structured events**
* All inputs are transformed into **events** before reaching the Core
* No traditional logs, only immutable facts

See [Event Architecture](event-architecture.md) for details.

### 4. Core as Cognitive Kernel

The ANIMA Core behaves as a **Cognitive Kernel**, supervising concurrent tasks rather than executing them directly.

See [Cognitive Kernel](cognitive-kernel.md) for details.

### 5. Lease-Based Authorization

All Core ↔ Module communication is gated by **cryptographic leases** over **gRPC with mTLS**.

See [Module Types and Leases](module-types-and-leases.md) for details.

---

## 🌍 OUTSIDE WORLD

```
[ User ]        [ Platforms / Hardware / APIs ]
```

No intelligence here. Just reality.

---

## 🟦 MODULE LAYER (Effectful Boundary)

> **Only layer that touches the real world**

```
┌───────────────────────────────────────────┐
│               MODULES                     │
│                                           │
│  • Discord Input Module                   │
│  • CLI Input Module                       │
│  • Microphone Input Module                │
│                                           │
│  • Discord Output Module                  │
│  • CLI Output Module                      │
│  • TTS Output Module                      │
│  • Live2D Output Module                   │
│                                           │
│  (APIs, hardware, streaming, devices)     │
└───────────────────────────────────────────┘
```

📌 **Module Rules:**

* Modules **capture** or **execute**
* Modules **do not think**
* Modules **do not decide**
* All modules run **out-of-process** from Core
* Communication over **gRPC with mTLS**

### Module Types

Every module declares one of three types:

* **Type I** — Ephemeral Private (lease-bound, single Core)
* **Type II** — Resident Private (long-lived, single Core)
* **Type III** — Resident Shared (multi-tenant, infrastructure-governed)

See [Module Types and Leases](module-types-and-leases.md) for full specifications.

---

## 🟨 ADAPTER LAYER (Pure Translation Ring)

> **First protective ring around the core**

```
┌───────────────────────────────────────────┐
│               ADAPTERS                    │
│                                           │
│  Input Adapters:                          │
│   • Platform events → input.nl            │
│   • System signals → input.system         │
│   • NLP module → input.semantic           │
│                                           │
│  Output Adapters:                         │
│   • Intent → Module Commands              │
│   • Schema validation                     │
│   • Lease enforcement                     │
│                                           │
│  (Pure, deterministic, no I/O)            │
└───────────────────────────────────────────┘
```

📌 **Adapter Rules:**

* Adapters **translate only**
* No side effects
* No memory access
* No permissions
* Run **out-of-process** from Core
* Validate against **JSON Schemas**

See [Adapter-Actuator Split](adapter-actuator-split.md) for details.

---

## 🔄 ACTUATOR LAYER (Execution)

> **Effectful execution under strict lease control**

Actuators:

* Execute real-world side effects (APIs, hardware, platforms)
* Enforce lease scope and interruption policies
* Emit observability events
* Contain zero Core logic

See [Adapter-Actuator Split](adapter-actuator-split.md) for details.

---

## 🟩 CAPABILITY RING (Declarative Power)

> **What the core is allowed to want**

```
┌───────────────────────────────────────────┐
│              CAPABILITIES                 │
│                                           │
│  • send_text                              │
│  • speak_audio                            │
│  • render_avatar                          │
│  • move_robot                             │
│                                           │
│  (Contracts, not implementations)         │
└───────────────────────────────────────────┘
```

📌 **Capability Rules:**

* Capabilities are symbolic contracts
* Seed + Security gate them
* They do not execute anything directly
* Declared via **JSON Schemas**
* Versioned and immutable

---

## 🧠 CORE (Cognitive Kernel)

> **The only place where decisions are made**

```
┌───────────────────────────────────────────┐
│            COGNITIVE KERNEL               │
│                                           │
│  🧩 Cortex (Mandatory)                    │
│    • Planning & Reasoning                 │
│    • Task Expansion                       │
│    • Capability Selection                 │
│                                           │
│  💬 Arcuate (Optional)                    │
│    • NLP Processing                       │
│    • input.nl → input.semantic            │
│    • Intent → Natural Language            │
│                                           │
│  🎯 Task Supervisor                       │
│    • Concurrent task management           │
│    • Interrupt routing                    │
│    • Span lifecycle                       │
│                                           │
│  Inputs:                                  │
│   • Structured Events                     │
│   • Memory Query Results                  │
│   • Seed Constraints                      │
│   • Security Policies                     │
│                                           │
│  Output:                                  │
│   • Intent Graphs                         │
│   • Task Directives                       │
│   • Observability Events                  │
└───────────────────────────────────────────┘
```

📌 **Core Rules:**

* Core **never touches the world**
* Core produces **intent**, not effects
* Core **supervises** work, doesn't execute it
* All actions are **interruptible by design**
* **Cortex** (cognition) is mandatory
* **Arcuate** (NLP) is optional

See [Cognitive Kernel](cognitive-kernel.md) and [AI Model Topology](ai-model-topology.md) for details.

---

## 🟦 INTERNAL CONTEXT (Influence, Not Control)

These surround the core but **do not execute**.

### 🧬 Seed (Static Identity)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Personality parameters │
│  • Tone / expressiveness  │
│  • Risk tolerance         │
│  • Allowed capabilities   │
│  • Identity boundaries    │
│  • Memory policies        │
└───────────────────────────┘
```

* Loaded at startup
* Immutable during runtime
* Data, not code
* Instance-defining but not instance-unique

See [Seed System](seed-system.md) for details.

---

### 🧠 Memory (Dynamic, Fallible)

```
┌───────────────────────────┐
│          MEMORY           │
│                           │
│  • Past interactions      │
│  • Observations           │
│  • Task states            │
│  • Confidence-weighted    │
│    facts                  │
│  • Instance-local only    │
└───────────────────────────┘
```

* Instance-local
* Queried via MTL (Medial Temporal Lobe, memory domain), never blindly trusted
* Layered (episodic, semantic, narrative)
* No cross-instance sharing

---

## 🔐 SECURITY & POLICY (Cross-Cutting)

```
┌───────────────────────────┐
│          SECURITY         │
│                           │
│  • Authentication         │
│  • Authorization          │
│  • Lease Management       │
│  • Permission enforcement │
│  • Dangerous action gates │
│  • Audit logging          │
└───────────────────────────┘
```

Security:

* Wraps **input before core**
* Validates **intent before execution**
* Issues and manages **cryptographic leases**
* Enforces **capability boundaries**

---

## 🔁 FULL FLOW (CLEAN & LINEAR)

```
User / Environment
 ↓
Input Module (capture)
 ↓
Adapter (translate to events)
 ↓
Events: input.nl / input.system / input.semantic
 ↓
Authentication / Security
 ↓
COGNITIVE KERNEL
  ↔ Memory (query)
  ↔ Seed (constraints)
  ↔ Cortex (reasoning)
  ↔ Arcuate (optional NLP)
 ↓
Intent + Capability URN
 ↓
Security (validate lease & permissions)
 ↓
Output Adapter (validate schema, map to command)
 ↓
Actuator (execute under lease)
 ↓
Effect + Observability Events
```

No shortcuts. No leaks. All steps observable and auditable.

---

## Event-Based Architecture

All system activity is expressed through **structured events**:

* **input.nl** — Natural language (pre-semantic)
* **input.system** — System/mechanical facts
* **input.semantic** — Post-interpretation asserted meaning
* **capability.invoked** — Capability execution started
* **capability.completed** — Capability execution finished
* **capability.interrupted** — Capability execution interrupted

Events are:
* Immutable
* Execution-scoped
* Causally traceable
* Concurrency-safe

See [Event Architecture](event-architecture.md) for details.

---

## Interruption & Multitasking

The Core operates as a **multitasking kernel** that supports:

* Concurrent task execution
* Cooperative interruption
* Explicit span management
* Policy-governed preemption

Interruptions are:
* First-class events
* Classified (override/cancel/queue/clarification/emergency)
* Policy-governed
* Observable and auditable

See [Cognitive Kernel](cognitive-kernel.md) for details.

---

## Domain-Driven Design

ANIMA follows strict **Hexagonal Architecture** rules:

```
infra  ─────▶  domains
runtime ────▶  infra + domains

domains ─╳──▶  infra
```

* Domains define **ports** (interfaces)
* Infrastructure implements **adapters**
* Domains contain **only business logic**
* Runtime assembles the system

See [Domain and Infrastructure](domain-and-infrastructure.md) for details.

---

## 🧠 Architectural Litmus Test 

Ask:

* Can I simulate everything without modules? → **Yes**
* Can I swap Discord for Slack without touching the core? → **Yes**
* Can I run multiple Seeds on the same engine? → **Yes**
* Can I audit intent before execution? → **Yes**
* Can interruptions be replayed from event logs? → **Yes**
* Can modules run without Core code loaded? → **Yes**
* Is all execution traceable and observable? → **Yes**

---

## Key Architectural Invariants

The following are **non-negotiable architectural constraints**:

1. **Engine is identity-agnostic** — personality comes from Seeds
2. **Core never loads third-party code** — modules run out-of-process
3. **All execution requires valid leases** — no lease, no execution
4. **Domains never import infrastructure** — only ports/interfaces
5. **All observability is event-based** — no traditional logs
6. **Memory is strictly instance-scoped** — no cross-instance sharing
7. **Cortex is mandatory, Arcuate is optional** — cognition vs. language
8. **Core supervises, doesn't execute** — kernel model
9. **All actions are interruptible** — cooperative interruption
10. **Events are the source of truth** — immutable, auditable facts

---

## Summary

ANIMA is a **cognitive kernel** that:

* Separates engine from identity
* Supervises behavior rather than executing it
* Communicates through structured events
* Enforces security through cryptographic leases
* Maintains clean architectural boundaries
* Enables long-lived, evolving identities

> **ANIMA does not execute instructions. It supervises behavior.**
> **ANIMA does not log text. It records facts.**
> **ANIMA is an engine for growing identities, not a single personality.**
