# Seed System — Identity Initialization in ANIMA

**Related ADR:** [ADR-001: Separation of ANIMA Engine and Identity via Seed System](../adr/ADR-001.md)

---

## Overview

The **Seed System** is ANIMA's mechanism for separating the engine from identity. A seed is a declarative configuration used to initialize an ANIMA identity, ensuring that the engine remains identity-agnostic while enabling diverse, customizable personalities.

---

## What is a Seed?

A **Seed** is a structured, data-driven blueprint that defines:

* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial self-concept and narrative framing
* Memory decay and promotion policies

A seed is **data, not code**. It configures the engine without modifying its logic.

---

## What a Seed Must NOT Include

Seeds are strictly initialization data. They MUST NOT contain:

* Learned memories
* User data
* Conversation logs
* Private behavior evolution
* Execution logic
* Cross-instance state

---

## Engine vs. Identity

### ANIMA Engine

The engine is a private runtime responsible for:

* Intent understanding and reasoning
* Task planning and long-lived task orchestration
* Memory mechanics (episodic, semantic, narrative)
* Capability discovery, permissioning, and execution
* Security, confirmation flows, and audit logging
* Adapter-agnostic input/output handling

The engine:

* Does not encode personality
* Does not contain hardcoded behavioral tone
* Does not store cross-instance memory
* Enforces all safety and permission boundaries

### ANIMA Identity (via Seed)

Each ANIMA instance is initialized with a seed and evolves independently using **instance-local memory only**.

Once initialized:

* The instance develops its own memories
* Behavior evolves within the constraints defined by the seed
* No cross-instance sharing or learning occurs

---

## Architectural Invariants

The following constraints are now architectural invariants:

* The engine must remain persona-agnostic
* Seeds may tune or restrict behavior but cannot extend execution power
* Capability enforcement occurs exclusively at the engine level
* Memory is strictly instance-scoped
* Identity evolution must be explicit and reviewable (no silent drift)

---

## Use Cases

### Private ANIMA Product

* Users license the engine runtime
* Users initialize with a chosen seed
* Users own their instance's data and memory
* Engine source code remains private

### Streaming / VTuber ANIMA (ANIMA Prime)

* Implemented as `ANIMA Engine + Prime Seed`
* Prime seed is protected IP and never distributed
* Shares no memory or identity state with customer instances

---

## Benefits

* Enables multiple independent ANIMA instances with shared engine logic
* Supports commercialization without distributing source code
* Prevents identity and memory leakage across instances
* Allows a protected "Prime" identity for streaming / VTuber use
* Makes personality fully data-driven and configurable
* Simplifies testing, upgrades, and engine evolution

---

## Future Work

* Define seed schema and versioning strategy
* Implement seed validation and compatibility checks
* Formalize engine upgrade guarantees across seed versions
* Define licensing and capability gating mechanisms
