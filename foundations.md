# 1️⃣ Project Charter

This document answers *“what exists”* and *“what is forbidden”*.

## Core Purpose

* Provide a **private, evolving AI runtime**
* Separate **engine** from **identity**
* Support **multiple incarnations** via Seeds
* Enable **safe, auditable interaction with the world**

## Non-Goals (Explicit)

* No self-modifying code
* No uncontrolled autonomy
* No internet access unless granted via capability
* No shared memory between instances
* No implicit user permissions

## Core Values

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

# 2️⃣ Canonical Glossary

These definitions must **never drift**.

## Engine

The entirety of the ANIMA system, containing:

* Core 
* Seed
* Memory
* Capabilities
* Modules
* Adapters
* Cortex

---

## Core

The reasoning loop inside the engine.

It:

* consumes input
* queries memory
* applies Seed constraints
* selects capabilities
* produces **intent**

---

## Seed (File)

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

## Memory

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

## Capability

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

## Module

An **effectful implementation** of a capability.

Modules:

* perform real-world actions
* talk to APIs, hardware, platforms
* never decide *when* or *why*
* only execute *what they’re told*

Modules are the **only** place where **Cause is detected** and **Effects are produced**.

---

## Adapter

A **pure translation layer** between representations.

Adapters:

* transform external input → core input
* transform core intent → module command
* contain no external I/O
* contain only translation logic
* are deterministic

Adapters exist to **protect the core from format pollution**.

---

## Intent

A structured description of **what should happen**, not how.

Produced by the core.  
Consumed by adapters and modules.  
Auditable, loggable, replayable.  
Contain what + when + where + how much + why + what to do if something goes wrong. Along with confidence scores.

---

## Task

A long-lived unit of work the engine undertakes. Solved with series of Intents.

Tasks:

* persist across time
* can pause / resume
* may invoke capabilities repeatedly
* are tracked in memory

---

## Cortex

The wrapper around a given AI model, connected to the engine for reasoning.

Cortexes:
* provide completion services
* are interchangeable without needing to change the engine
* are replaceable

---

## Package

A distributable group of modules, adapters, and capability definitions.
Can be installed into an ANIMA instance to extend functionality in bulk.

Packages:

* bundle related capabilities
* include adapters for those capabilities
* are versioned
* can be shared

---

## Semantic Spine

An explicit data structure for semantic representation of a message expected to be passed to user or received from user. 

Semantic Spines are used to ensure consistent and meaningful communication between the engine and users, providing a standardized way to represent the meaning and context of messages.

Semantic Spines:
* encapsulate intent and context
* facilitate accurate interpretation
* are language-agnostic
* support complex interactions
* enable better memory encoding

# 3️⃣ System Boundaries

## What the engine can *never* do
* Directly perform side effects
* Modify its own code or Seed
* Access the internet without explicit capability
* Share memory between instances
* Bypass permission checks

## What must *always* require confirmation
* Accessing sensitive user data
* Executing high-risk capabilities (e.g., financial transactions, physical actions)
* Handling destructive commands (e.g., deleting data, shutting down systems)
* Replacing data
* Non-read-only irreversible actions

## What is delegated to modules

* All external I/O operations
* API calls
* Hardware interactions
* Intaking user commands and information
* Executing capability commands

---
