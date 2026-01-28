# Seed System — Identity Initialization in ANIMA

**Related ADR:** [ADR-001: Separation of ANIMA Engine and Identity via Seed System](../adr/ADR-001.md)

---

## Overview

The **Seed System** is ANIMA's mechanism for separating the engine from identity. A Seed is a declarative configuration used to initialize an ANIMA Identity, ensuring that the engine remains identity-agnostic while enabling diverse, customizable personalities.

**Key Concepts:**
* **ANIMA Seed** - Memoryless identity priors (portable, declarative configuration)
* **ANIMA Identity** - Seed + Memory (persistent, evolves over time)
* **ANIMA Instance** - Running execution that hosts an Identity (ephemeral)

**Canonical Sentence:** "An ANIMA Instance executes an ANIMA Identity derived from an ANIMA Seed and Memories."

---

## What is an ANIMA Seed?

An **ANIMA Seed** is a structured, data-driven blueprint that defines identity priors:

* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial self-concept and narrative framing
* Memory decay and promotion policies

A Seed is **data, not code**. It configures the engine without modifying its logic.

A Seed is **memoryless** - it contains no learned experiences, conversation history, or evolved behavior.

---

## What is an ANIMA Identity?

An **ANIMA Identity** is a materialized identity composed of:
* **ANIMA Seed** (identity priors and configuration)
* **Memory** (episodic, semantic, and narrative layers that evolve over time)

An Identity is **persistent** across Instance executions and **evolves** through experience.

---

## What is an ANIMA Instance?

An **ANIMA Instance** is a single running execution of the ANIMA engine.

An Instance is **ephemeral** - it exists only during runtime and is destroyed on shutdown.

An Instance **hosts** an ANIMA Identity (Seed + Memory) and provides the execution lifecycle.

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
* MTL (memory storage, retrieval, decay, promotion mechanics)
* Capability discovery, permissioning, and execution
* Security, confirmation flows, and audit logging
* Adapter-agnostic input/output handling

The engine:

* Does not encode personality
* Does not contain hardcoded behavioral tone
* Does not store cross-Identity memory
* Enforces all safety and permission boundaries

### ANIMA Identity (via Seed + Memory)

Each ANIMA Identity is initialized with a Seed and evolves independently using **identity-local memory only**.

Once initialized:

* The Identity develops its own memories
* Behavior evolves within the constraints defined by the Seed
* No cross-Identity sharing or learning occurs

---

## Architectural Invariants

The following constraints are now architectural invariants:

* The engine must remain persona-agnostic
* Seeds may tune or restrict behavior but cannot extend execution power
* Capability enforcement occurs exclusively at the engine level
* Memory is strictly identity-scoped
* Identity evolution must be explicit and reviewable (no silent drift)

---

## Use Cases

### Private ANIMA Product

* Users license the engine runtime
* Users initialize with a chosen Seed
* Users own their Identity's data and memory
* Engine source code remains private

### Streaming / VTuber ANIMA (ANIMA Prime Identity)

* Implemented as `ANIMA Instance + Prime Identity (Prime Seed + Prime Memory)`
* **Prime Seed is protected IP and NEVER distributed**
* **Prime Memory is NEVER exported or shared**
* **Prime Identity cannot be instantiated by third parties**
* Shares no memory or identity state with user Identities

**Critical Constraints for ANIMA Prime Identity:**
* Non-exportable - Seed and Memory are protected IP
* Non-cloneable - Cannot be duplicated or forked
* Non-shareable - No public access to Prime Seed or Memory
* Restricted instantiation - Only authorized systems can load Prime Identity

---

## Benefits

* Enables multiple independent ANIMA Identities with shared engine logic
* Supports commercialization without distributing source code
* Prevents identity and memory leakage across Identities
* Allows a protected "Prime Identity" for streaming / VTuber use
* Makes personality fully data-driven and configurable
* Simplifies testing, upgrades, and engine evolution

---

## Future Work

* Define seed schema and versioning strategy
* Implement seed validation and compatibility checks
* Formalize engine upgrade guarantees across seed versions
* Define licensing and capability gating mechanisms
