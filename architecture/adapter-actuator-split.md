# Adapter–Actuator Split (Module-Owned, Out-of-Process)

**Related ADRs:**
* [ADR-003: Core ↔ Module Communication Protocol, Module Types, and Lease Lifecycle](../adr/ADR-003.md)
* [ADR-004: Observability, Event Logging, and Execution Traceability](../adr/ADR-004.md)
* [ADR-005: Interruption & Preemption Model](../adr/ADR-005.md)
* [ADR-010: Adapter–Actuator Split with Strict Process Boundary](../adr/ADR-010.md)
* [ADR-011: Event-Based Input Architecture](../adr/ADR-011.md)

---

## Overview

This document specifies how Modules are structured to protect the Core from external code while preserving clean contracts.

Every Module is divided into two parts, both running **out-of-process** from the Core:

* **Adapter** — Pure translation and schema validation
* **Actuator** — Effectful execution that touches APIs/hardware

The Core **never loads or executes module-owned code**. All Core ↔ Module interaction occurs over **gRPC with mTLS** and **Core-issued leases**.

---

## Core Security Principle

> **The Core runs only trusted, engine-owned code.**

* Adapters and Actuators run in module-owned processes
* All communication uses gRPC over mTLS with attested identities (Core URN, Module URN) and Core-issued leases
* No dynamic imports, plugins, or Python execution from modules inside Core

---

## Module Package Structure

Module package example:

```
module-package/
├── adapter/
│   ├── input/  (signals → input.* events; pure mapping)
│   └── output/ (intent → commands; pure mapping + schema validation)
├── actuator/ (effectful execution: APIs/hardware)
├── capability_contract/ (JSON Schemas + manifest)
├── transport/ (gRPC server/client; mTLS; lease enforcement)
└── proto/ (IDL for CapabilityGateway and Adapter services)
```

---

## Adapter (Module-side, Pure Translation)

### Responsibilities

**Input path:**
* Maps captured signals → Core input events

**Output path:**
* Maps Core Intent (capability URN + payload) → module command schema

### Key Duties

* Deterministic translation
* JSON Schema validation against the capability contract
* Lease scope enforcement on outbound calls

### Prohibitions

* No planning, no policy
* No external I/O beyond gRPC to Core or local actuator invocation
* No memory access or Core internals

---

## Actuator (Module-side, Effectful Execution)

### Responsibilities

* Executes real-world side effects (APIs, hardware, platforms)
* Enforces lease scope and interruption policies (ADR-005)
* Emits observability events with ADR-004 envelope
* Contains zero Core logic or reasoning

---

## Inbound Flow (Inputs → Core)

1. Module runtime captures signals (text, audio, platform events)
2. Adapter maps signals to input events with ADR-004 envelope
3. Adapter publishes events via gRPC
4. Core ingests events (never raw IO)

Event types (see [Event Architecture](event-architecture.md)):
* `input.nl` — natural language (pre-semantic)
* `input.system` — non-linguistic, mechanical/system facts
* `input.semantic` — post-interpretation asserted meaning

---

## Outbound Flow (Core Intent → Module Command)

1. Core produces an Intent with capability URN + payload
2. Core calls the capability gateway on the Module Adapter
3. Adapter validates payload against capability JSON Schema
4. Adapter maps to Actuator command
5. Adapter enforces lease scope
6. Adapter invokes Actuator
7. Actuator executes and emits events

---

## Natural Language Processing in Modules

Modules that deal with natural language input and output MAY either ship an NLP implementation or delegate NLP responsibilities to the Core's Arcuate (see [AI Model Topology](ai-model-topology.md)).

### Natural Language Input

**Option A — Module-owned NLP:**
* The module's Adapter performs local NLP to transform `input.nl` → `input.semantic` events
* Constraints:
  * Translation only; no planning or policy
  * Include confidence and provenance in semantic payloads
  * No Core memory access; provenance reflects "module"

**Option B — Delegate to Arcuate:**
* The module's Adapter publishes `input.nl` events to Core
* The Core's Arcuate consumes `input.nl` and produces `input.semantic`
* Core reasons on semantic events
* Constraints:
  * Adapter remains pure; no embedded NLP
  * Process boundary preserved; no plugin code in Core

### Natural Language Output (e.g., TTS, chat delivery)

**Option A — Module-owned NLG:**
* The module's Adapter maps Core intents to output command schemas and performs local NLG (surface realization) before Actuator execution
* Constraints:
  * NLG is translation; no planning, no memory access
  * Observability events MUST capture intent, realization metadata, and execution outcome

**Option B — Delegate to Arcuate:**
* The Core produces the linguistic surface via Arcuate from an intent's semantic content
* The module's Adapter translates the realized text into the module's command
* The Actuator executes (e.g., speak/send)
* Constraints:
  * Adapter stays pure; Actuator performs side effects
  * Arcuate usage follows ADR-009 isolation rules

In all cases:
* Adapters remain pure mapping + validation
* Actuators perform IO and effects under valid leases
* Semantic events MUST include confidence and provenance fields
* Natural language MUST NOT trigger direct execution; the pipeline remains Input → Interpretation → Intent → Validation/Confirmation → Execution

---

## Discovery & Trust

Modules present a manifest with:
* Module URN
* Capability URNs and versions
* JSON Schema hashes
* Declared Module Type (see [Module Types and Leases](module-types-and-leases.md): Type I/II/III)

Core validates manifest over mTLS at handshake; **no code loaded from Module**.

All requests carry LeaseID/Epoch; stale epochs are rejected.

---

## Protocol & Contracts

### Inbound (Module → Core)
* Adapter publishes input events and event types

### Outbound (Core → Module)
* Core calls Adapter via a generic Capability Gateway:
  * `capability_urn` + `schema_version` + `payload` (validated by adapter)
  * `LeaseMeta` (lease_id, epoch)
  * `ExecMeta` (execution/trace/span)

### Capability Contract

Declarative JSON Schemas for command payloads containing:
* Method side-effect class (reversible/irreversible)
* Interruption policy (soft-stop/hard-stop/checkpoint/non-interruptible)
* Maximum lease duration

---

## Observability

Both Adapter and Actuator emit structured events per ADR-004:

Event types:
* `capability.invoked`
* `capability.completed`
* `capability.interrupted`
* `validation.failed`

All events include:
* `execution_id`, `trace_id`, `span_id`, `thread_id`, `timestamp`
* `instance_urn`, `core_urn`
* `source=module:<name>`
* `type=<event.type.identifier>`
* `payload=<structured fields only>`

No free-text logging as authority; events are the source of truth.

---

## Security Invariants

* No plugin code in Core — ever
* All module execution requires a valid lease with current epoch
* Scope promotion/demotion occurs only via Core-issued lease updates
* Zero-Lease State: modules are inert (Type II standby or Type I terminate)
* Adapters cannot widen scope or bypass Core gating

---

## Why This Is Safe

* Core processes events only, never third-party code
* Translation and execution are out-of-process
* Declarative schemas prevent ad-hoc payloads
* Leases and epochs prevent replay/escalation
* Eliminates code injection risk in Core
* Preserves hexagonal boundaries and testability
* Clear responsibility split (translate vs execute)
* Strong auditability via structured events

---

## Summary

> **Core reasons and supervises. Modules adapt and act.**
> **No third-party code runs inside Core.** 