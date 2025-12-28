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
│      Adapters       │  (Intention to command conversion)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Capability Modules │  (tools, streaming, robots, etc)
└────────────────────┘
```

The engine has **no knowledge of platforms, personalities, or embodiment**.

---

## The Seed System

A **Seed** defines an ANIMA identity.

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

## FAQ

### What is ANIMA, exactly?

ANIMA is a **private, modular AI engine** designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

What that means is that you can create your own unique AI identity by providing a **Seed** and give it specific capabilities, while ensuring that it operates safely and respects your privacy.

It is **not**:

* a chatbot
* a single personality
* an autonomous agent that acts on its own

ANIMA is an *engine for growing identities*, not a prepackaged character.

---

### Where can I find the ANIMA engine?

The ANIMA engine lives in a **separate repository** called `engine-core`.

That repository is currently **not public** while core safety, memory, and identity boundaries are still under development.

This public docs repository exists to:

* explain the architecture
* document design decisions
* make the long-term vision transparent

Once the engine reaches stable phases, more parts of the ecosystem may be opened.

---

### How do I download or run ANIMA?

At the moment, ANIMA is **not yet available for download**.

When released:

* ANIMA will be distributed as an engine runtime
* users will initialize their own private instances using approved seeds
* memory will always remain instance-local

Details about installation and supported platforms will be published closer to release.

---

### Will ANIMA be free once released?

The current plan is a **mixed model**:

* Some components (documentation, seed formats, adapters) may be public
* Running the ANIMA engine itself will likely require a license or subscription
* Certain curated seeds may be premium

ANIMA is designed as a **long-term system**, not a one-off app, and sustainability is a core consideration.

Exact pricing is not finalized yet.

---

### Is ANIMA safe to use?

Safety is a **core architectural requirement**, not an afterthought.

ANIMA enforces:

* explicit permissions
* capability allow/deny lists
* human confirmation for dangerous actions
* audit logging
* strict separation between identity and execution

If ANIMA cannot safely perform an action, it must explain *why* and refuse.

---

### Can ANIMA delete my files?

Not unless you explicitly allow it.

ANIMA:

* has **no access to your system by default**
* cannot execute capabilities it does not have
* cannot escalate permissions on its own

Any capability that can affect files, devices, or systems must be:

1. explicitly installed
2. explicitly allowed by the seed
3. explicitly permitted by the user by at least two separate channels
4. confirmed at runtime if dangerous

---

### Can ANIMA spy on me, record me, or access the internet?

No — not by default.

ANIMA:

* does not have internet access unless a capability provides it
* does not record audio or video unless an adapter is installed
* does not share memory across instances
* does not upload your data to other users’ instances
* does not monitor you passively unless you explicitly ask it to

If ANIMA can perceive something, it must be because you **explicitly connected** that input and told her to use it.

---

### Does ANIMA share memory between users?

No. Never.

Each ANIMA instance:

* has its own isolated memory
* evolves independently
* cannot access or influence other instances

There is no cross-user learning or shared “global memory” beyond model training.

---

### Will ANIMA hallucinate or invent things?

ANIMA is designed to **avoid silent hallucinations**.

The engine tracks whether information is:

* observed
* remembered
* inferred
* unknown

If ANIMA is unsure, it is expected to say so.

Perfect accuracy is not guaranteed, but **confident fabrication is treated as a bug**.

---

### Is ANIMA an autonomous agent?

No.

ANIMA does not:

* self-deploy
* self-modify its core
* pursue goals without user involvement
* act without explicit intent and permission

Long-lived tasks (like streaming or monitoring) exist, but they are:

* user-initiated
* interruptible
* auditable
* constrained by permissions

---

### What is a “Seed”?

A seed is a **structured identity blueprint** used only at initialization.

It defines:

* personality parameters
* behavioral constraints
* allowed capabilities
* initial narrative framing

A seed does **not** include:

* memories
* conversation history
* learned behavior
* execution logic

After initialization, each instance grows its own identity through experience.

---

### What is ANIMA Prime?

ANIMA Prime is a **future protected internal identity** used for streaming / VTuber embodiment.

It will run on the same engine as all other instances but uses a private, non-distributed seed.

ANIMA Prime will not be sold or shared.

---

### Is ANIMA open source?

Parts of the ANIMA ecosystem may be open.

The engine core itself is currently **closed** to ensure:

* identity safety
* memory integrity
* controlled evolution of capabilities

This may change over time, but the engine will never be released in a way that compromises user trust or privacy.

---

### When will ANIMA be released?

ANIMA is under active development, but there is **no fixed release date** yet.
The current focus is on:
* Core engine correctness
* Memory integrity
* Capability boundaries
* Seed validation
* Safety features

Future announcements will be made as milestones are reached and stable phases are approached.
Demos will be shared when appropriate.

---

### Why is development slow?

Intentionally so.

ANIMA prioritizes:

* correctness over speed
* safety over novelty
* long-term trust over short-term demos

Some things are easy to build quickly — and very hard to fix later.
ANIMA is being built to last.

---

### How can I follow development?

Updates, architectural decisions, and philosophy are documented in this repository.

Major milestones will be announced once the engine reaches stable phases.

Announcements will be added in the `/announcements` folder and shared via future official channels.