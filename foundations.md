# 🧭 ANIMA — PHASE 0: FOUNDATIONS

---

## 🎯 Goal

Establish **ANIMA as an identity engine**, not a specific personality.

ANIMA is:

* a **private AI engine**
* capable of **long-lived identity**
* extensible through **capabilities and modules**
* shaped at runtime by a **Seed file**

ANIMA is **not**:

* a chatbot
* an autonomous agent that self-expands
* an automation platform
* a monolithic AI with hardcoded behavior
* an internet-crawling system by default

---

## 🧱 Build (Phase 0 Deliverables)

### 1️⃣ Project Charter

This document answers *“what exists”* and *“what is forbidden”*.

#### Core Purpose

* Provide a **private, evolving AI runtime**
* Separate **engine** from **identity**
* Support **multiple incarnations** via Seeds
* Enable **safe, auditable interaction with the world**

#### Non-Goals (Explicit)

* No self-modifying code
* No uncontrolled autonomy
* No internet access unless granted via capability
* No shared memory between instances
* No implicit user permissions

#### Core Values

* **Truth over confidence**  
  *What does this mean?* The system prioritizes accuracy and honesty in its responses, even if it means admitting uncertainty or lack of knowledge.
* **Intent over execution**  
  *What does this mean?* The system focuses on understanding and fulfilling the user's intentions rather than just executing commands blindly.
* **Modularity over monolith**  
  *What does this mean?* The system is designed with interchangeable components, allowing for flexibility and adaptability rather than being a single, unchangeable entity.
* **Safety over capability**  
  *What does this mean?* The system prioritizes user safety and ethical considerations above expanding its functionalities or capabilities.
* **Configurability over hardcoding**  
  *What does this mean?* The system emphasizes the ability to be customized and configured through external settings rather than having fixed, hardcoded behaviors.
* **Isolation over convenience**
  *What does this mean?* The system values keeping components and processes separate to enhance security and reliability, even if it sacrifices some ease of use.

This charter is your *constitutional law*.
Every feature must be justifiable against it.

---

### 2️⃣ Canonical Glossary

These definitions must **never drift**.

#### Engine

The runtime system that:

* receives normalized input
* reasons
* plans
* enforces identity, memory, permissions, and policy

The engine **never performs side effects**.

---

#### Core

The reasoning loop inside the engine.

It:

* consumes input
* queries memory
* applies Seed constraints
* selects capabilities
* produces **intent**

---

#### Seed (File)

A **static configuration artifact** loaded at initialization.

Defines:

* personality parameters
* behavioral constraints
* tone and expressiveness ranges
* allowed capabilities
* identity boundaries
* risk tolerance

A Seed:

* does **not** contain memories
* does **not** change itself
* does **not** contain code

---

#### Memory

Instance-local data describing:

* past interactions
* observations
* task states
* confidence-weighted facts

Memory:

* informs reasoning
* never overrides policy
* is fallible and queryable

---

#### Capability

A **declarative contract** describing *what the core is allowed to intend*.

Example:

* `send_message`
* `move_robot`
* `start_stream`

Capabilities:

* contain no logic
* contain no I/O
* are permission-gated
* are Seed-restricted

---

#### Module

An **effectful implementation** of a capability.

Modules:

* perform real-world actions
* talk to APIs, hardware, platforms
* never decide *when* or *why*
* only execute *what they’re told*

Modules are the **only** place where **Cause is detected** and **Effects are produced**.

---

#### Adapter

A **pure translation layer** between representations.

Adapters:

* transform external input → core input
* transform core intent → module command
* contain no external I/O
* contain only translation logic
* are deterministic

Adapters exist to **protect the core from format pollution**.

---

#### Intent

A structured description of **what should happen**, not how.

Produced by the core.  
Consumed by adapters and modules.  
Auditable, loggable, replayable.  
Contain what + when + where + how much + why + what to do if something goes wrong. Along with confidence scores.

---

#### Task

A long-lived unit of work the engine undertakes. Solved with series of Intents.

Tasks:

* persist across time
* can pause / resume
* may invoke capabilities repeatedly
* are tracked in memory

---

### 3️⃣ **System Boundaries**

#### What the engine can *never* do
* Directly perform side effects
* Modify its own code or Seed
* Access the internet without explicit capability
* Share memory between instances
* Bypass permission checks

#### What must *always* require confirmation
* Accessing sensitive user data
* Executing high-risk capabilities (e.g., financial transactions, physical actions)
* Handling destructive commands (e.g., deleting data, shutting down systems)
* Replacing data
* Non-read-only irreversible actions

#### What is delegated to modules

* All external I/O operations
* API calls
* Hardware interactions
* Intaking user commands and information
* Executing capability commands

---

## ✅ Exit Criteria (Do NOT Advance Without These)

**You can explain ANIMA in 2 minutes without mentioning personality**

ANIMA is a private AI engine designed to host long-lived, evolving AI identities safely.

At its core, ANIMA separates thinking, identity, and action.

The core is the only part that reasons. It takes structured input, consults memory, applies identity constraints from a Seed file, checks permissions, and produces intent—never direct actions.

A Seed is a static identity definition: personality parameters, behavioral boundaries, risk tolerance, and which capabilities are allowed. It doesn’t contain memories or code. Each ANIMA instance grows independently after initialization.

Memory is instance-local and fallible. It stores past interactions, task states, and observations, and informs decisions without overriding policy.

The core can only act through capabilities, which are declarative contracts describing what it is allowed to do, not how. Capabilities are gated by both the Seed and security rules.

When the core produces intent, adapters translate that intent into concrete commands. Adapters are pure and deterministic—they don’t do external I/O or make decisions.

Actual interaction with the world happens only in modules. Modules talk to APIs, hardware, platforms, or streams, and execute commands without reasoning.

Security wraps the system end-to-end: authentication before reasoning, and policy enforcement before execution.

This design allows ANIMA to support private assistants, stream personas, robots, and tools—all using the same engine—while keeping identity isolated, behavior auditable, and actions safe.

---

## ⚠️ Phase 0 Traps 

* Writing code without clear separation of concerns
* Letting modules decide behavior
* Letting memory override policy
* Blurring Seed vs Memory
* Treating adapters as optional


