# ANIMA — Frequently Asked Questions

This document answers common questions about ANIMA. For deeper architectural details, see the linked canonical documentation.

---

## General Questions

### What is ANIMA, exactly?

ANIMA is a **private, modular AI engine** designed to host long-lived, evolving artificial identities under strict safety, memory, and capability constraints.

ANIMA enables you to create your own unique AI identity by providing a **Seed** (declarative configuration) and granting specific capabilities, while ensuring the system operates safely and respects your privacy.

**ANIMA is:**
* An engine for growing identities over time
* Private by design (each instance owns its own memory)
* Safety-first (capability gating, lease-based authorization)
* Modular (inputs, outputs, and capabilities are pluggable)

**ANIMA is not:**
* A chatbot or conversational wrapper
* A single fixed personality
* An autonomous agent that acts independently
* A shared-memory or hive-mind system

**Related:** [Vision](vision/vision.md), [ANIMA Architecture](architecture/anima-architecture.md)

---

### Where can I find the ANIMA engine?

The ANIMA engine lives in a separate repository called `engine-core`.

That repository is currently **not public** while core safety, memory, and identity boundaries are under development.

This public documentation repository exists to:
* Explain the architecture transparently
* Document design decisions
* Make the long-term vision reviewable

Once the engine reaches stable phases, more parts of the ecosystem may be opened.

**Related:** [Roadmap](roadmaps/roadmap.md)

---

### How do I download or run ANIMA?

At the moment, ANIMA is **not yet available for download**.

When released:
* ANIMA will be distributed as an engine runtime
* Users will initialize their own private instances using approved Seeds
* Memory will always remain instance-local
* Capabilities must be explicitly granted

Details about installation and supported platforms will be published closer to release.

**Related:** [Project Viability](governance/project-viability.md)

---

### Will ANIMA be free once released?

The current plan is a **mixed model**:

* **ANIMA Engine (Runtime):** Open source (likely MIT or Apache 2.0)
* **ANIMA Prime Seed:** Available under specific terms (TBD)
* **Third-party Seeds:** Created and distributed by users or vendors

The goal is to make the engine freely available while allowing identity diversity and user autonomy.

**Related:** [Licensing Model](governance/licensing-model.md)

---

## Conceptual Questions

### What is a "Seed"?

A **Seed** is a static, declarative configuration file that defines an ANIMA identity.

**A Seed contains:**
* Personality parameters (tone, verbosity, emotional expressiveness)
* Behavioral policies (risk tolerance, uncertainty handling)
* Capability allow/deny lists
* Default interaction style
* Initial self-concept and narrative framing
* Memory decay and promotion policies

**A Seed must NOT contain:**
* Learned memories
* Conversation logs
* User data
* Execution logic
* Cross-instance state

After initialization, each ANIMA instance develops **instance-local memory only**. The Seed provides the starting configuration, not the ongoing experience.

**Related:** [Seed System](architecture/seed-system.md), [ADR-001](adr/ADR-001.md)

---

### What is ANIMA Prime?

**ANIMA Prime** is the **first canonical identity** designed for the ANIMA Engine.

Unlike private instances, ANIMA Prime is designed to be:
* **Streaming** - updates may be released over time
* **Protected** - the Seed is not publicly distributed
* **Reference implementation** - demonstrates ANIMA's design philosophy

**ANIMA Prime is not:**
* The only possible identity
* Required to use the engine
* A hive-mind shared across instances

Each instance of ANIMA Prime, even if initialized from the same Seed version, evolves independently based on its own experiences.

**Private instances** can be created with entirely different Seeds, personalities, and behavioral policies.

**Related:** [Seed System](architecture/seed-system.md), [Vision](vision/vision.md)

---

### How does ANIMA differ from ChatGPT, Claude, or other LLM assistants?

ANIMA is architecturally different from typical conversational AI systems:

| Aspect | Typical LLM Assistants | ANIMA |
|--------|------------------------|-------|
| **Identity** | Fixed personality per model | Configurable via Seed, evolves over time |
| **Memory** | Ephemeral context window | Layered, persistent, decay-aware memory |
| **Safety** | Prompt-based guardrails | Architectural capability gating and leases |
| **Continuity** | Resets between sessions | Long-lived, continuous identity |
| **Privacy** | Shared training/fine-tuning | Strictly instance-local, no cross-instance learning |
| **Capabilities** | Broad, implicit permissions | Explicit, declarative, auditable authorization |

ANIMA is optimized for **long-term identity continuity, trust through consistency, and strict safety boundaries**, not just intelligent-sounding responses.

**Related:** [Why Not an Agent?](vision/why-not-an-agent.md), [Vision](vision/vision.md)

---

### Why can't ANIMA just be replaced by an existing agent framework?

Most agent frameworks optimize for:
* Maximum capability and autonomy
* Task completion and efficiency
* Immediate utility

ANIMA optimizes for:
* Long-term identity continuity
* Trust through consistency and honesty
* Strict safety boundaries
* Private, evolving relationships

**Key architectural differences:**

1. **Long-Lived Identity vs. Ephemeral Sessions**
   * Agents: Reset between sessions, simulate memory through retrieval
   * ANIMA: Continuous, evolving identity with intentional memory decay

2. **Engine ≠ Identity (Separation)**
   * Agents: Personality baked into prompts/training
   * ANIMA: Engine and identity completely separate via Seed System

3. **Safety as Architecture vs. Safety as Prompt**
   * Agents: Guardrails through prompts and fine-tuning
   * ANIMA: Capability gating, lease-based authorization, cryptographic proof for execution

ANIMA is not a more capable agent; it's a different category of system optimized for reliability over utility maximization.

**Related:** [Why Not an Agent?](vision/why-not-an-agent.md), [Safety Model](safety/safety-model.md)

---

### Is ANIMA a RAG (Retrieval-Augmented Generation) system?

No. While ANIMA has a sophisticated memory system, it is not a RAG system.

**RAG systems:**
* Retrieve documents from external sources
* Use retrieval to augment generation context
* Typically stateless between sessions

**ANIMA's memory:**
* Tracks episodic experiences (what happened)
* Maintains semantic knowledge (what is known)
* Supports narrative memory (identity continuity)
* Uses layered memory with intentional decay policies
* Persists across sessions as part of identity

ANIMA's memory is **identity-centric**, not document-centric. It serves continuity and trust, not just information retrieval.

**Related:** [Memory Integrity](safety/memory-integrity.md), [ADR-003](adr/ADR-003.md)

---

## Architecture Questions

### What does "hexagonal architecture" mean for ANIMA?

ANIMA uses **hexagonal architecture** (also called ports-and-adapters):

* The **Core** (Cognitive Kernel) has no knowledge of platforms, personalities, or embodiment
* All I/O happens through **Modules** (out-of-process components)
* **Adapters** translate intent to platform-specific commands
* **Actuators** execute commands with effects

This means:
* The engine is platform-agnostic
* You can plug in different input sources (text, voice, Discord, sensors)
* You can plug in different capabilities (tools, robots, streaming services)
* The core reasoning remains identical regardless of embodiment

**Related:** [ANIMA Architecture](architecture/anima-architecture.md), [ADR-002](adr/ADR-002.md)

---

### What are "capabilities" in ANIMA?

**Capabilities** are declarative contracts that define what an ANIMA instance can do.

Every action in ANIMA:
* Must be declared in a capability contract
* Requires explicit authorization (capability lease)
* Can be inspected, denied, or revoked
* Leaves an audit trail

**Capabilities are not:**
* Implicitly granted
* Hidden in prompts
* Automatically enabled
* Cross-instance shared

Users must explicitly grant capabilities. ANIMA cannot use a capability without a valid lease.

**Related:** [ADR-004](adr/ADR-004.md), [Module Types and Leases](architecture/module-types-and-leases.md)

---

### How does ANIMA prevent hallucination?

ANIMA treats hallucination as a **bug, not a feature**.

**Architectural safeguards:**
* Memory queries return **bounded, verified slices** (not full context dumps)
* The Core reasons over structured events, not raw text
* Actions require **cryptographic leases** - hallucinated capabilities fail at execution
* Uncertain knowledge is marked explicitly (epistemic state tracking)
* Execution happens in lease-gated modules, not the reasoning core

ANIMA cannot execute an action she does not have permission for, even if she "thinks" she can. The architecture enforces this.

**Hallucination in responses** is mitigated through:
* Explicit uncertainty markers
* Separation of speculation from assertion
* Validation against memory and capability contracts

**Related:** [Safety Model](safety/safety-model.md), [Cognitive Kernel](architecture/cognitive-kernel.md)

---

## Safety & Privacy Questions

### How does ANIMA ensure safety?

ANIMA enforces safety **architecturally**, not through prompts:

1. **Capability Gating** - No action without explicit authorization
2. **Lease-Based Execution** - Cryptographic proof required for all effects
3. **Human Confirmation** - High-risk actions require user approval
4. **Process Isolation** - Modules run out-of-process, cannot corrupt the Core
5. **Execution Boundaries** - The Core produces intent, not effects
6. **Audit Logging** - All actions are observable and traceable

Safety constraints are **enforced at the engine level**, not delegated to personality or user expectation.

**Related:** [Safety Model](safety/safety-model.md), [Threat Model](safety/threat-model.md)

---

### Is my data shared across ANIMA instances?

**No.** ANIMA instances are **strictly isolated**.

**Privacy guarantees:**
* Each instance has private, instance-local memory
* No shared learning pool
* No cross-instance memory access
* No global conversation logs
* No personality data extraction

When you run an ANIMA instance, **you own its memory** and evolution. It does not learn from other users, and other users do not learn from you.

**Related:** [Non-Goals](vision/non-goals.md), [Memory Integrity](safety/memory-integrity.md)

---

### Can ANIMA act autonomously on the internet?

**No.** ANIMA is explicitly **not** designed to operate as an autonomous internet agent.

**Constraints:**
* No free web browsing by default
* No self-directed online actions without explicit approval
* Any internet interaction is capability-gated
* Context-limited and fully auditable

Unsupervised online autonomy is treated as a **high-risk behavior**, not a feature.

**Related:** [Non-Goals](vision/non-goals.md), [Safety Model](safety/safety-model.md)

---

### Does ANIMA monitor or surveil users?

**No.** ANIMA is not a surveillance system.

**Prohibitions:**
* No passive monitoring of people, conversations, or environments
* No recording without explicit consent
* No behavioral data aggregation across users or instances
* No background tracking

ANIMA's design prioritizes **privacy and locality**. Observation only occurs when intentionally initiated and clearly bounded.

**Related:** [Non-Goals](vision/non-goals.md), [Safety Model](safety/safety-model.md)

---

## Development & Community Questions

### Why is development slow?

ANIMA development prioritizes **correctness over speed**.

Safety boundaries, memory integrity, and identity separation are foundational. These cannot be bolted on later without architectural compromise.

The project deliberately:
* Publishes architecture decisions before implementation
* Makes the vision reviewable before release
* Builds safety constraints into the core, not as afterthoughts
* Values long-term viability over early demos

**Related:** [Project Viability](governance/project-viability.md), [Vision](vision/vision.md)

---

### Can I contribute to ANIMA?

The ANIMA engine (`engine-core`) is currently private during foundational development.

**You can contribute to:**
* This documentation repository (propose improvements, fix errors)
* Design discussions (when community channels open)

When the engine reaches stable phases, contribution guidelines will be published.

**Related:** [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md)

---

### When will ANIMA be released?

There is no fixed release date.

ANIMA will be released when:
* Core safety boundaries are validated
* Memory integrity is provably sound
* Capability gating is cryptographically secure
* The Seed System is stable

Rushing release would compromise the foundational principles that make ANIMA different from existing systems.

**Related:** [Roadmap](roadmaps/roadmap.md), [Project Viability](governance/project-viability.md)

---

## Additional Resources

* **For new readers:** [GETTING_STARTED.md](GETTING_STARTED.md)
* **For architectural deep-dive:** [ANIMA Architecture](architecture/anima-architecture.md)
* **For safety review:** [Safety Model](safety/safety-model.md), [Threat Model](safety/threat-model.md)
* **For design rationale:** [ADRs](adr/) (read in order, ADR-001 through ADR-011)
* **For terminology:** [Canonical Glossary](foundations/canonical-glossary.md)

---

**Last Updated:** January 28, 2026
