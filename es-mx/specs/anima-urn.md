# ANIMA URN Specification

**Authority:** ANIMA  
**Version:** 0.1.0

---

## 1. Purpose

This document defines the canonical structure, semantics, and rules for
**ANIMA Uniform Resource Names (URNs)**.

ANIMA URNs provide:
- Globally unique identifiers
- Stable, immutable identity
- Explicit semantic authority
- Long-lived references independent of location or implementation

This specification is **normative**.  
All implementations MUST conform to the rules defined here.

---

## 2. Design Principles

ANIMA URNs are designed according to the following principles:

1. **Immutability**  
   Once issued, a URN MUST NEVER change meaning.

2. **Authority-Centric Identity**  
   Meaning is governed by the set of rules, specifications, and trust assumptions that define how an ANIMA URN is interpreted, the aforementioned ANIMA authority, not by tools or code.

3. **Explicit Semantics**  
   All semantic interpretation MUST be derivable from the URN structure and associated specifications.

4. **Versioned Contracts**
    Each URN defines a version of the identity it represents. Version changes imply distinct identity semantics.

5. **Opaque Identifiers**
    The final identifier segment MUST NOT encode semantics. It serves solely as a unique reference.

6. **Central Specification**
    This document is the single source of truth for ANIMA URN structure and semantics. Generator implementations are secondary.


---

## 3. Canonical Format

The canonical ANIMA URN format is:

```

urn:anima:<scope>:<namespace>@<version>:<id>

```

### 3.1 Segment Overview

| Segment     | Description |
|------------|-------------|
| `urn`      | Standard URN scheme identifier |
| `anima`    | Root namespace authority |
| `scope`    | Trust and lifecycle boundary |
| `namespace`| Semantic identity domain |
| `version`  | Semantic identity contract version |
| `id`       | Opaque unique identifier |

---

## 4. Scope

The `scope` segment defines the **authority and lifecycle guarantees** of
the identified resource.

Allowed values:

- `core`  
  Canonical, first-class identities recognized by the ANIMA runtime itself.

- `module`  
  Identities owned by extensions, plugins, or external modules.

Examples:

```

urn:anima:core:seed@0.1.0:...
urn:anima:module:CLI.capability@0.2.1:...

```

---

## 5. Namespace

The `namespace` segment defines the **semantic domain** of the identity.

### 5.1 Rules

- MUST be lowercase
- MUST use alphanumeric characters and dots (`.`)
- MUST NOT contain colons (`:`)
- SHOULD reflect conceptual hierarchy, not implementation structure

### 5.2 Examples

```

seed
cortex.reasoning
schema.intent
runtime.instance

```

---

## 6. Versioning

The `version` segment specifies the **semantic identity contract** governing
the interpretation of the URN.

### 6.1 Version Format

Versions MUST follow strict semantic versioning:

```

<major>.<minor>.<patch>

```

Example:
```

0.0.2
1.0.0
2.3.1

```

### 6.2 Semantic Meaning

Within a URN:

- ANY version change represents a **distinct identity semantic**
- Compatibility MUST NOT be inferred from version numbers alone
- Even patch-level changes (`0.0.1` → `0.0.2`) imply a new identity class

Compatibility relationships, if any, MUST be declared explicitly outside
the URN.

---

Excellent catch — this is a **very important correction**, and you’re absolutely right to enforce it.
Allowing content hashes *does* quietly break opacity, even if people pretend it doesn’t.

Here’s a **clean, spec-consistent rewrite** that fully preserves opaque identity.

---

## 7. Identifier (`id`)

The final segment is an **opaque unique identifier**.

### 7.1 Allowed Forms

- UUID v4

The identifier MUST:
- Be globally unique
- Be opaque and non-derivable
- NOT encode semantics, structure, versioning, or content information
- NOT be deterministically derived from the resource it identifies

Cryptographic hashes, checksums, or any identifier derived from resource content
MUST NOT be used as URN identifiers, as they violate the opacity requirement.

### 7.2 Rationale (Informative)

Opaque identifiers ensure that:
- Identity is independent from representation
- Identifiers remain stable despite resource changes
- No assumptions can be inferred from the identifier itself

Examples:

```

6f9619ff-8b86-d011-b42d-00cf4fc964ff
9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1

```
---

## 8. Immutability Rules

Once issued:

- A URN MUST remain valid indefinitely
- A URN MUST NOT be repurposed
- A URN MUST NOT change meaning
- A URN MUST NOT be reissued with different semantics

Any semantic evolution requires issuing a **new URN** with a new version.

---

## 9. Generator Implementations

URN generators are **implementations of this specification**, not
authoritative definitions.

- Generator version numbers MUST NOT appear in URNs
- Multiple generators MAY coexist
- All generators MUST produce URNs compliant with this document

This specification is the single source of truth.

---

## 10. Validation (Informative)

A valid ANIMA URN MUST match the following *conceptual* pattern:

```

urn:anima:(core|module):[a-z0-9.]+@[0-9]+.[0-9]+.[0-9]+:<opaque-id>

```

---

## 11. Examples

### Core Seed Instance
```

urn:anima:core:seed@0.0.2:6f9619ff-8b86-d011-b42d-00cf4fc964ff

```

### Core Reasoning Cortex
```

urn:anima:core:cortex.reasoning@0.1.0:9a31c4b2-4f2d-4e9e-a7f9-8b32d3e1f9a1

```

### Schema Identity
```

urn:anima:core:schema.intent@1.0.0:ba3bf79c-381e-409b-9450-12e241100d3a

```

---

## 12. Future Extensions

Future revisions MAY define:
- Explicit compatibility declarations
- Signature binding rules
- Resolution mechanisms
- Cross-authority federation

Such extensions MUST NOT violate the immutability guarantees defined here.

---

## 13. Final Notes

ANIMA URNs are **identity contracts**, not references, not locations,
and not implementation artifacts.

They exist to preserve meaning across time, systems, and interpretations.