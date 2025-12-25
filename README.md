# ANIMA
![Status](https://img.shields.io/badge/status-active%20development-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** is a private, modular AI engine designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

It is not a chatbot, not a prompt wrapper, and not a single personality.

ANIMA is an **engine for growing identities**.

---

## Core Principles

* **Engine ≠ Identity**
  Reasoning and execution are strictly separated from personality and behavior.

* **Private by Design**
  Each ANIMA instance owns its memory and evolves independently.

* **Safety over Capability**
  Permissions, confirmations, and boundaries are enforced at the engine level.

* **Modularity First**
  Inputs, outputs, capabilities, and embodiment are pluggable.

* **Continuity over Recall**
  ANIMA prioritizes consistent identity and trust over perfect memory.

---

## Architecture Overview

ANIMA follows a hexagonal / ports-and-adapters architecture.

```
┌────────────────────┐
│   Input Modules    │  (text, voice, discord, sensors)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  ANIMA Engine   │
│                    │
│  - Reasoning       │
│  - Planning        │
│  - Task System     │
│  - Memory Layers   │
│  - Security        │
│  - Capabilities    │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│      Adpters       │  (Intention to command convertion)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Capability Modules │  (tools, streaming, robots, etc)
└────────────────────┘
```

The engine has **no knowledge of platforms, personalities, or embodiment**.

---

## The Seed System

A **Seed** defines a ANIMA identity.

A seed is **data**, not code.

### A seed may define:

* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial narrative framing
* Memory decay and promotion rules

### A seed must not include:

* Learned memories
* Conversation logs
* User data
* Execution logic
* Cross-instance state

After initialization, each ANIMA instance develops **instance-local memory only**.

---

## ANIMA Prime vs User Instances

* **ANIMA Prime**
  A protected identity used for streaming / VTuber embodiment.
  Implemented as:

  ```
  ANIMA Engine + Prime Seed
  ```

  The Prime seed is never distributed.

* **User ANIMA Instances**
  Private instances initialized with user-selected seeds.
  Users own their instance’s memory and evolution.

All instances run on the **same engine**, but remain fully isolated.

---

## Memory Model

ANIMA uses layered memory to balance realism, safety, and cost:

* **Working Memory**
  Short-lived, current and recent tasks and context.

* **Episodic Memory**
  Short-lived, high-fidelity interaction context.

* **Semantic Memory**
  Atomic facts and preferences stored as embeddings with confidence metadata.

* **Narrative Memory**
  Curated, meaningful events that preserve identity continuity.

ANIMA tracks whether knowledge is:

* observed
* remembered
* inferred
* unknown

She never silently guesses.

---

## Capabilities

Capabilities are explicit, discoverable modules.

The engine:

* reasons about capabilities
* checks permissions
* requests confirmation when required
* executes in a controlled sandbox

Seeds may **restrict** capabilities, but never grant new execution power.

---

## Security & Permissions

* Stable user identities
* Role-based access
* Capability-level permission checks
* Human confirmation for dangerous actions
* Full audit logging

ANIMA explains *why* she can or cannot act.

---

## What ANIMA Is Not

* ❌ A general-purpose autonomous agent
* ❌ An internet-connected AI by default
* ❌ A shared-memory system
* ❌ A personality hardcoded into prompts
* ❌ A replacement for human judgment

---

## Project Status

ANIMA is under active development.

Current focus:

* Core engine correctness
* Memory integrity
* Capability boundaries
* Seed validation

This project is intentionally evolving slowly.

---

## Design Documents

Key architectural decisions are tracked as ADRs in `/docs/adr`:

* ADR-001: Engine / Identity Separation via Seed System

---

## Philosophy

ANIMA is not designed to feel intelligent at all costs.

She is designed to feel **consistent, honest, and safe to grow with**.
