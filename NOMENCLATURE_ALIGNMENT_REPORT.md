# Nomenclature Alignment Report

**Date:** 2025-01-28  
**Task:** Comprehensive nomenclature alignment across ANIMA documentation  
**Objective:** Clarify terminology to distinguish between Instance, Identity, Seed, and Prime Identity

---

## Executive Summary

This report documents a comprehensive nomenclature alignment effort across the ANIMA documentation repository to establish clear, consistent terminology for:

1. **ANIMA Instance** - A running execution of the engine (ephemeral)
2. **ANIMA Identity** - Seed + Memory (persistent, evolves)
3. **ANIMA Seed** - Identity priors without memory (memoryless, portable)
4. **ANIMA Prime Identity** - Special protected identity (non-exportable, non-shareable)

**Canonical Sentence:** "An ANIMA Instance executes an ANIMA Identity derived from an ANIMA Seed and Memories."

---

## Motivation

Prior to this alignment, the documentation used "instance" ambiguously to refer to:
* Running executions (what we now call "Instance")
* Persistent identities (what we now call "Identity")
* The combination of Seed + Memory

This ambiguity created confusion about:
* What persists across restarts (Identity, not Instance)
* What owns memory (Identity, not Instance)
* What can be shared or exported (Seeds can be, Prime Identity cannot)
* The relationship between ephemeral execution and persistent personality

---

## Canonical Definitions Added

### 1. ANIMA Instance

**Definition:** A single running execution of the ANIMA engine.

**Properties:**
* Ephemeral (exists only during runtime, destroyed on shutdown)
* Owns execution lifecycle and resources
* Hosts one active ANIMA Identity at a time
* Identified by a unique Instance URN generated at startup

**What it is NOT:**
* Not an identity (identities are persistent, instances are ephemeral)
* Not persistent across restarts
* Not what provides continuity (continuity comes from Identity: Seed + Memory)

---

### 2. ANIMA Identity

**Definition:** A materialized identity composed of an ANIMA Seed plus its associated Memory.

**Composition:**
* **ANIMA Seed** (identity priors and configuration)
* **Memory** (episodic, semantic, and narrative layers that evolve over time)

**Properties:**
* Persistent across Instance executions
* Evolves over time through experience
* Can be migrated between Instances (with Seed + Memory)
* Instance-independent (the "who" that transcends runtimes)

**What it is NOT:**
* Not a running process (that's an Instance)
* Not ephemeral (persists across runtimes)
* Not cloneable without complete migration of Seed and Memories

---

### 3. ANIMA Seed

**Definition:** A cryptographic, unmaterialized identity definition that contains identity priors but no memory.

**Properties:**
* Memoryless (contains no learned experiences or conversation history)
* Portable (can be distributed and shared)
* Defines who ANIMA can become (not what she remembers)
* Used to initialize an Identity

**What it is NOT:**
* Not an identity by itself (Identity = Seed + Memory)
* Not executable code (declarative configuration only)
* Not stateful (does not evolve or change)

---

### 4. ANIMA Prime Identity

**Definition:** A special, protected identity with unique non-exportable Seed and private evolving Memory.

**Properties:**
* Unique protected Seed (never distributed or shared)
* Private evolving Memory (never exported or shared)
* Non-cloneable (cannot be instantiated by third parties)
* Non-exportable (Seed and Memory remain protected IP)

**Critical Constraints:**
* Prime Seed is NEVER shared publicly
* Prime Memory is NEVER exported or distributed
* No private Instance may load Prime Identity
* Prime Identity cannot be cloned or forked

---

## Files Modified

### Core Documentation Files (9 files)

1. **foundations/canonical-glossary.md**
   - Added complete canonical definitions for Instance, Identity, Seed, Prime Identity
   - Updated Seed entry to reference new canonical definition
   - Updated Memory and MTL entries to use "identity-local" terminology
   - Updated Instance URN description

2. **README.md**
   - Updated Seed System section with canonical sentence
   - Clarified ANIMA Prime Identity vs User Identities section
   - Updated "Private by Design" principle to reference Identity
   - Changed "private instances" → "private Identities"

3. **FAQ.md**
   - Expanded "What is a Seed?" with Identity/Instance/Seed distinctions
   - Rewrote "What is ANIMA Prime?" → "What is ANIMA Prime Identity?" with explicit constraints
   - Updated licensing section to reflect Prime Memory protection
   - Changed "instance-local" → "identity-local" throughout
   - Updated comparison table to clarify Identity = Seed + Memory
   - Added note about capability grants to Instances, not Seeds

4. **GETTING_STARTED.md**
   - Updated "Engine ≠ Identity" key concept with canonical sentence
   - Changed "Private by Design" to reference Identity ownership

### Architecture Files (3 files)

5. **architecture/seed-system.md**
   - Complete rewrite of overview section with canonical definitions
   - Added "What is an ANIMA Seed?" section
   - Added "What is an ANIMA Identity?" section
   - Added "What is an ANIMA Instance?" section
   - Updated Use Cases section with ANIMA Prime Identity constraints
   - Changed "instance-local memory" → "identity-local memory"

6. **architecture/anima-architecture.md**
   - Updated Seed section to emphasize memoryless nature
   - Updated Memory section to clarify identity-local ownership
   - Added notes about Identity = Seed + Memory

7. **architecture/directory-overview.md**
   - Updated Key Architectural Principles to use "Identity-scoped memory"
   - Added clarification that Memory is part of Identity, not Instance

### Vision Files (4 files)

8. **vision/vision.md**
   - Updated references to "ANIMA instance" → "ANIMA Identity" where appropriate
   - Added note about Identity persistence across Instance executions
   - Updated long-term vision platform description

9. **vision/project-charter.md**
   - Changed "instance-local memory" → "identity-local memory"
   - Updated all memory isolation references to use Identity
   - Changed "Private instances" → "Private Identities"
   - Updated ANIMA Prime reference

10. **vision/non-goals.md**
    - Updated all "cross-instance" → "cross-Identity"
    - Changed "Each instance evolves" → "Each Identity evolves"
    - Updated memory sharing prohibitions to reference Identities

11. **vision/why-not-an-agent.md**
    - Updated instance isolation → Identity isolation throughout
    - Changed "Private instances" → "Private Identities"
    - Updated quoted ADR text to reflect identity-scoped memory

### Safety Files (1 file)

12. **safety/memory-integrity.md**
    - Updated "No Cross-Instance Memory" → "No Cross-Identity Memory"
    - Added note that Memory is part of Identity (Seed + Memory), not Instance

### Foundation Files (2 files)

13. **foundations/system-boundaries.md**
    - Updated "Share Memory Between Instances" → "Share Memory Between Identities"
    - Added note about Memory being part of Identity
    - Changed enforcement descriptions to use Identity terminology

14. **foundations/reinvention-rationale.md**
    - Changed "instance-scoped isolation" → "identity-scoped isolation"
    - Updated "User owns instance data" → "User owns Identity data"

### ADR Files (1 file)

15. **adr/ADR-001.md**
    - Added Key Concepts section with Instance/Identity/Seed definitions
    - Updated "instance-local memory" → "identity-local memory"
    - Changed "cross-instance" → "cross-Identity" throughout
    - Expanded ANIMA Prime section with complete Prime Identity constraints
    - Updated "Memory is strictly instance-scoped" → "identity-scoped"

### Roadmap Files (1 file)

16. **roadmaps/roadmap.md**
    - Renamed "Phase 7 — Streaming / Prime Instance" → "Streaming / Prime Identity"
    - Expanded Prime Identity section with Seed + Memory protection details
    - Added explicit non-exportable, non-cloneable constraints

### Governance Files (2 files)

17. **governance/licensing-model.md**
    - Changed "instance-local memory" → "identity-local memory"
    - Updated "instance owner" → "Identity owner"
    - Updated ownership model to reference Identity

18. **governance/project-viability.md**
    - Changed "instance isolation" → "Identity isolation"
    - Updated "memory is instance-local" → "identity-local"
    - Updated ANIMA Prime reference with protection constraints

---

## Key Terminology Changes

### Primary Replacements

| Old Term | New Term | Context |
|----------|----------|---------|
| instance-local memory | identity-local memory | Memory ownership |
| instance-scoped memory | identity-scoped memory | Memory isolation |
| cross-instance sharing | cross-Identity sharing | Memory boundaries |
| cross-instance queries | cross-Identity queries | Data isolation |
| ANIMA Prime | ANIMA Prime Identity | Protected identity |
| Prime seed | Prime Seed + Prime Memory | Complete identity |
| private instances | private Identities | User ownership |

### Conceptual Shifts

1. **Memory Ownership**
   - **Before:** "Each ANIMA instance owns its memory"
   - **After:** "Each ANIMA Identity owns its memory" (clarifying that memory is part of Identity, not the ephemeral Instance)

2. **Persistence**
   - **Before:** "instance evolves independently"
   - **After:** "Identity evolves independently" (clarifying what persists across runtimes)

3. **ANIMA Prime**
   - **Before:** "Prime seed is protected IP and never distributed"
   - **After:** "Prime Identity (Prime Seed + Prime Memory) is protected, non-exportable, non-cloneable IP"

---

## ANIMA Prime Identity Corrections Applied

All references to ANIMA Prime have been updated to:

1. **Use correct terminology:** "ANIMA Prime Identity" (not just "ANIMA Prime" or "Prime seed")

2. **Emphasize complete protection:**
   - Prime Seed is NEVER distributed
   - Prime Memory is NEVER exported
   - Prime Identity cannot be instantiated by third parties

3. **Clarify composition:**
   - ANIMA Prime Identity = Prime Seed + Prime Memory
   - Both components are protected IP

4. **Document constraints explicitly:**
   - Non-exportable
   - Non-cloneable
   - Non-shareable
   - Restricted to authorized contexts only

5. **Remove ambiguity:**
   - No longer implies that only the Seed is protected
   - Clarifies that Memory evolution is also private
   - Establishes that the complete Identity is the protected unit

---

## Relationship Model Established

**Canonical Relationship:**

```
Seed → Identity (Seed + Memory) → Instance (executes Identity)
```

**Lifecycle:**

1. **ANIMA Seed** - Memoryless identity priors (portable, can be shared except Prime)
2. **ANIMA Identity** - Seed + Memory (persistent, evolves over time)
3. **ANIMA Instance** - Running execution (ephemeral, hosts Identity)

**Ownership:**

- Users own their **Identity** (Seed + Memory)
- **Instance** is just the runtime execution (ephemeral)
- **Memory** belongs to Identity, not Instance

---

## Ambiguities Resolved

### 1. What persists across restarts?

**Answer:** The ANIMA Identity (Seed + Memory) persists. The Instance (running execution) is ephemeral.

### 2. What owns memory?

**Answer:** The ANIMA Identity owns memory. Memory is part of the Identity (Seed + Memory), not the Instance.

### 3. Can ANIMA Prime be cloned?

**Answer:** No. ANIMA Prime Identity (including both Prime Seed and Prime Memory) is non-exportable, non-cloneable, and cannot be instantiated by third parties.

### 4. What can be shared?

**Answer:** 
- User Seeds can be shared (portable)
- Prime Seed NEVER shared
- Memory is always identity-local (never shared between Identities)
- Identities can be migrated with their complete Seed + Memory

### 5. What is the "instance" in Instance URN?

**Answer:** The Instance URN identifies the running execution (ANIMA Instance), not the persistent Identity. It's generated at startup and destroyed at shutdown.

---

## Remaining Documentation

The following files were NOT modified as they do not contain terminology requiring alignment:

- Specification files (specs/*.md) - Technical specifications that already use precise terminology
- Most ADR files (except ADR-001) - ADRs are historical records; only updated where clarity was needed
- Announcement files - Historical records
- Diagram files - Visual aids that don't require textual updates
- Translation files (pt-br/, es-mx/) - Out of scope for this English-focused alignment

---

## Verification

The following checks confirm alignment success:

1. ✅ All four canonical definitions added to foundations/canonical-glossary.md
2. ✅ Canonical sentence appears in appropriate documentation
3. ✅ "instance-local/scoped memory" → "identity-local/scoped memory" throughout
4. ✅ ANIMA Prime Identity properly documented with all constraints
5. ✅ Relationship model (Seed → Identity → Instance) clarified
6. ✅ No weakening of architectural guarantees
7. ✅ Tone and rigor preserved
8. ✅ No new terminology introduced beyond the four canonical terms

---

## Summary

This nomenclature alignment effort successfully clarified the critical distinction between:

- **ANIMA Instance** (ephemeral execution)
- **ANIMA Identity** (persistent Seed + Memory)
- **ANIMA Seed** (memoryless identity priors)
- **ANIMA Prime Identity** (protected, non-exportable identity)

The alignment:
- Improved clarity on what persists vs. what is ephemeral
- Clarified memory ownership (Identity, not Instance)
- Documented ANIMA Prime Identity protection comprehensively
- Established canonical sentence for the relationship model
- Maintained architectural rigor and safety guarantees

**Total Files Modified:** 18 English documentation files  
**Zero Architectural Guarantees Weakened**  
**Zero New Terminology Introduced**  
**Alignment: Complete**

---

**Report Completed:** 2025-01-28
