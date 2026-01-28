# MEMORY_INTEGRITY.md

## Purpose

This document explains **how ANIMA remembers** — and just as importantly, **why she forgets**.

Memory integrity is central to trust.
Perfect recall, unlimited storage, and unfiltered memory are not features — they are failure modes.

ANIMA treats memory as a **controlled system**, not a log file.

---

## Why Perfect Recall Is Bad

Human trust depends on **continuity**, not completeness.

Perfect recall creates several problems:

* ❌ Context overload
* ❌ False authority (“I remember everything”)
* ❌ Inability to adapt
* ❌ Privacy risk
* ❌ Increased hallucination surface

A system that remembers *everything* must also decide:

* what matters
* what is relevant
* what can be safely inferred

Most systems do this **implicitly** and **silently**.

ANIMA does not.

---

## Memory Is Not a Transcript

ANIMA does **not** store conversations verbatim by default.

Raw logs are:

* noisy
* misleading
* emotionally unweighted
* dangerous when misinterpreted later

Instead, ANIMA uses **layered memory**, each with a clear purpose, scope, and lifecycle.

---

## Memory Layers

### 1. Working Memory (Immediate Context)

**What it is:**

* Current task state
* Active goals
* Short-term variables
* Ongoing plans and tool usage

**Properties:**

* Extremely short-lived
* Actively mutated
* Cleared or replaced continuously

**Why it exists:**

* Enables multi-step reasoning
* Supports long-lived tasks
* Allows coherent action sequencing

Working memory is **purely operational**.
It is never persisted as long-term memory.

---

### 2. Episodic Memory (Interaction Context)

**What it is:**

* High-fidelity interaction context
* Recent conversational history
* Local emotional and situational cues

**Properties:**

* Volatile
* Session-bound
* Automatically discarded or compressed

**Why it exists:**

* Enables natural conversation flow
* Prevents constant re-asking
* Supports short-term coherence

Episodic memory is **not trusted long-term**.

---

### 3. Semantic Memory (Facts & Preferences)

**What it is:**

* Atomic facts
* User preferences
* Stable, reusable information

Stored using:

* Structured entries
* Embeddings for retrieval
* Confidence and provenance metadata

Each item tracks whether it was:

* observed
* explicitly stated
* inferred
* uncertain

**Why embeddings are used:**

* Efficient recall
* Approximate relevance
* Cost control

Embeddings are **assistive**, not authoritative.

---

### 4. Narrative Memory (Identity Continuity)

**What it is:**

* Curated, meaningful events
* Relationship-defining moments
* Commitments and turning points

Narrative memory is:

* Sparse
* Intentional
* Explicitly promoted

Examples:

* “User trusts ANIMA with long-term planning.”
* “A conflict occurred and was resolved.”
* “A boundary was clearly established.”

**Why narrative memory matters:**

Identity is not built from facts — it’s built from *stories*.

This is how ANIMA remains **recognizably the same entity over time**.

---

## How Summaries and Embeddings Are Used

ANIMA summarizes **to preserve meaning, not wording**.

### Summaries:

* Reduce noise
* Capture intent
* Preserve emotional weight
* Avoid misleading detail

### Embeddings:

* Enable approximate recall
* Support relevance-based retrieval
* Prevent brittle keyword matching

Important constraint:

> **Neither summaries nor embeddings are treated as ground truth.**

They are *retrieval aids*, not memory itself.

---

## Hallucination Resistance Through Memory Design

Hallucinations often come from:

* Overconfident recall
* Misinterpreted summaries
* Missing provenance

ANIMA mitigates this by:

* Tracking confidence per memory item
* Distinguishing fact from inference
* Refusing to fill gaps silently
* Treating uncertain recall as uncertain

If memory confidence is too low:

* ANIMA asks
* or refuses
* or says “I don’t know”

---

## Memory Decay

Forgetting is intentional.

### Why Memory Decays

* Prevents identity ossification
* Reduces false assumptions
* Limits long-term error accumulation
* Mirrors human trust dynamics

### How Memory Decays

* Working memory is always ephemeral
* Episodic memory always decays
* Semantic memory decays unless reinforced
* Narrative memory decays the slowest

Decay may be triggered by:

* Time
* Inactivity
* Contradiction
* Explicit correction

Nothing is silently erased without reason.

---

## No Cross-Identity Memory

Each ANIMA Identity owns its memory.

* No shared memory pools
* No training on user data
* No identity leakage
* Memory is identity-local (bound to Seed + Memory, not shared)

What one ANIMA Identity learns stays with that Identity.

**Note:** Memory is part of an ANIMA Identity (Seed + Memory), not the Instance (running execution).

---

## What ANIMA Will Never Do

ANIMA will not:

* Claim perfect recall
* Invent memories
* Merge memories across users
* Pretend summaries are transcripts
* Use memory to override user corrections

Memory exists to support trust — not authority.

---

## Summary

ANIMA's MTL (memory domain) prioritizes:

* Integrity over volume
* Continuity over completeness
* Transparency over confidence
* Identity over optimization

She is not designed to remember everything.

She is designed to remember **what matters** — and to be honest about the rest.
