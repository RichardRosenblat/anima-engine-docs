# ANIMA — Development Roadmap

**Version:** 1.0
**Scope:** Engine, Seed System, Instances, Productization
**Guiding Principle:** *Engine ≠ Identity ≠ Memory*
**Current Phase:** Phase 1 — Core Engine Skeleton (Identity-Free)

---

## Phase 0 — Foundations (Do Not Skip)

### 🎯 Goal

Define what ANIMA *is* and *is not*.

### 🧱 Build

1. **Project Charter**

   * Core purpose (private, evolving AI engine)
   * Non-goals (no uncontrolled autonomy, no internet by default)
   * Core values (truth over confidence, safety over capability)

2. **Glossary**

   * Engine
   * Seed
   * Instance
   * Memory
   * Capability
   * Adapter

3. **System Boundaries**

   * What the engine can *never* do
   * What must *always* require confirmation
   * What is delegated to modules

### ✅ Exit Criteria

* You can explain ANIMA in 2 minutes **without mentioning personality**
* You can diagram Engine / Seed / Instance on a whiteboard

---

## Phase 1 — Core Engine Skeleton (Identity-Free)

### 🎯 Goal

Create a personality-agnostic reasoning OS.

### 🧠 Build

* Core reasoning loop
* Intent → Plan → Action pipeline
* Capability registry (empty at first)
* Adapter interface (input/output abstraction)
* Task abstraction (but not persistence yet)

### 🚫 Explicitly Avoid

* Opinions
* Tone
* Personality
* “I feel” language

### ✅ Exit Criteria

* Engine can receive input and choose actions
* No hardcoded behavior beyond safety rules
* Engine works identically regardless of context

---

## Phase 2 — Seed System 


### 🎯 Goal

Make identity a *boot-time concern*, not a runtime mutation.

### 🧬 Build

1. **Seed Schema (v1.0)**

   * Personality parameters
   * Behavioral constraints
   * Capability policy
   * Initial narrative framing

2. **Seed Validation**

   * Schema validation
   * Signature verification
   * Version compatibility

3. **Engine ↔ Seed Contract**

   * Engine reads seed
   * Engine never mutates seed
   * Engine enforces seed-defined constraints

### 🔐 Security

* Seeds are read-only after initialization
* Tampered seeds fail hard

### ✅ Exit Criteria

* Engine runs with different seeds **without code changes**
* Same input + same memory + different seed → different behavior
* Seed is never consulted as “memory”

---

## Phase 3 — Instance & Memory Architecture

### 🎯 Goal

Allow ANIMA to *grow* without identity drift.

### 🧠 Build

1. **Instance Lifecycle**

   * Create instance from engine + seed
   * Initialize empty memory
   * Bind adapters

2. **Memory Layers**

   * working memory (ephemeral)
   * episodic memory (short-term)
   * semantic memory (long-term facts)
   * narrative memory (identity continuity)

3. **Memory Write Rules**

   * What can be stored
   * Who can trigger writes
   * Confirmation for sensitive memory

### 💡 Important

* Memory belongs to the *instance*, not the seed
* No cross-instance reads. Ever.

### ✅ Exit Criteria

* Restarting an instance preserves identity continuity
* Two instances with same seed feel different after working memory diverges

---

## Phase 4 — Capability System & Gating

### 🎯 Goal

Make power explicit, auditable, and controllable.

### 🧩 Build

1. **Capability Interface**

   Working examples:
   * Name
   * Risk level
   * Required permissions
   * License requirements

2. **Execution Pipeline**
   * Capability lookup
   * Permission checks
   * Execution sandboxing
   * Logging & auditing


3. **Danger Classification**  

   Examples:
   * Safe
   * Sensitive
   * Dangerous

### 🔒 Examples

* Robot control = dangerous
* File access = sensitive
* Chat = safe

### ✅ Exit Criteria

* Engine cannot execute actions without passing the gate
* Capabilities can be added/removed without touching core logic

---

## Phase 5 — Task System (Long-Lived Consciousness)

### 🎯 Goal

Allow persistent, inspectable activities.

### 🕰️ Build

* Persistent tasks
* Task pause/resume
* Task ownership & permissions
* Safe shutdown & recovery

### 🧠 Examples

* Streaming loop
* Monitoring chat
* Long-term research task

### ✅ Exit Criteria

* Tasks survive restarts
* Tasks respect capability gating
* Tasks can be inspected and canceled

---

## Phase 6 — Adapter Ecosystem

### 🎯 Goal

Adapters abstract input/output without leaking logic.

### 🔌 Build

* Text adapter
* Voice adapter
* Discord adapter
* (Later) Streaming adapter (OBS / VTuber)
* (Later) Robot adapter

### 🔑 Rules

* Adapters never contain logic
* Adapters never bypass permissions
* Adapters are swappable

### ✅ Exit Criteria

* Same instance works across multiple adapters
* No adapter-specific behavior leaks into the engine

---

## Phase 7 — Streaming / Prime Identity

### 🎯 Goal

Create a special ANIMA Identity for streaming (ANIMA Prime Identity).

### 🌟 Build

* Prime Identity (Prime Seed + Prime Memory, protected IP)
* Prime Seed (signed, restricted, NEVER distributed)
* Prime Memory (NEVER exported or shared)
* Streaming adapter
* Public-safe capability set
* Strong moderation policies

### 🚫 Explicit Rule

No special-case code.
If streaming needs it, *everyone* gets the abstraction.

### ✅ Exit Criteria

* Streaming ANIMA uses same engine
* Prime Identity cannot be instantiated outside authenticated context
* Prime Seed and Memory remain protected (non-exportable, non-cloneable)
---

## Phase 8 — Licensing & Productization

### 🎯 Goal

Make ANIMA sustainable.

### 💳 Build

* License verification service
* Offline grace periods
* Capability-tier mapping
* Seed marketplace support

### 🧠 Sell

* Engine access
* Capability unlocks
* Curated seeds
* Updates & support

### ✅ Exit Criteria

* Unlicensed engine still works (limited)
* Licensing only gates *power*, not identity

---

## Ethical and Environmental Transition Roadmap

### Phase 1 - Foundation (Current)

**Focus:** Transparent third-party usage with ethical evaluation

**Deliverables:**

* ✅ Document all third-party model dependencies
* ✅ Establish ethical evaluation criteria for third-party models
* ✅ Prefer providers with clear ethical policies
* ✅ User data privacy architecturally enforced
* ✅ Local-first architecture implemented
* 🔄 Regular review of third-party model choices
* 🔄 Public documentation of model selection reasoning

**Timeline:** Ongoing during active development

---

### Phase 2 - Transition (Growing Resources)

**Focus:** Begin shift to first-party models and code

**Prerequisites:**

* Sufficient funding for model training or fine-tuning
* Team capacity for first-party development
* Infrastructure for ethical data sourcing and verification

**Deliverables:**

* 🎯 Develop first-party Cortex implementations
* 🎯 Train or fine-tune models on verifiably ethical data sources
* 🎯 Document complete training data provenance
* 🎯 Reduce reliance on third-party models
* 🎯 Establish environmental impact metrics
* 🎯 Optimize first-party code for energy efficiency
* 🎯 Provide migration paths from third-party to first-party modules

**Timeline:** When resources are acquired

---

### Phase 3 - Maturity (Long-term Goal)

**Focus:** Complete ethical control and environmental leadership

**Deliverables:**

* 🎯 Fully first-party Cortex and Arcuate with complete provenance
* 🎯 All training data legally accessed and consent-based
* 🎯 Zero reliance on ethically uncertain sources
* 🎯 Full transparency and public documentation
* 🎯 Minimal environmental footprint
* 🎯 Complete control over energy costs
* 🎯 First-party modules where critical
* 🎯 Leadership in ethical AI development and efficient design

**Timeline:** Long-term vision

---

### Commitment Statement

> **The shift to first-party models and code is ANIMA's first priority when resources are acquired.**
>
> **This is not a deferred goal or optional enhancement — it is a constitutional commitment.**
>
> **Current constraints are temporary. The commitment to improvement is permanent.**

---

## Phase 9 — Cost Control & Optimization

### 🎯 Goal

Keep ANIMA affordable to run.

### 💸 Build

* Token budgeting
* Memory summarization + embeddings (with raw fallback)
* Task throttling
* Instance sleep / wake

### ✅ Exit Criteria

* Predictable monthly cost
* No runaway memory growth
* User-visible cost transparency

---

## Phase 10 — Refinement & Evolution

### 🎯 Goal

Let ANIMA grow safely.

### 🌱 Build

* Seed version upgrades
* Memory reflection tools
* Introspection reports
* Controlled evolution paths

### ✅ Exit Criteria

* Users understand *why* ANIMA behaves as she does
* Changes feel organic, not random
