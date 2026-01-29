# Documentation Maintenance Guide

**Purpose:** This guide explains how ANIMA documentation is maintained, reviewed, and updated.

---

## Guiding Principles

ANIMA documentation follows these core principles:

1. **Clarity First** - Precision and comprehensibility over brevity
2. **Canonical Terminology** - Definitions in the [Canonical Glossary](foundations/canonical-glossary.md) must never drift
3. **No Authority Duplication** - Each concept has one authoritative source
4. **Architectural Honesty** - Document decisions as they are, not as we wish they were
5. **Cross-Reference Deliberately** - Link to canonical sources rather than repeating content

---

## Documentation Review Cadence

### Recommended Schedule

* **Quarterly Reviews** - Check for:
  * Broken cross-references
  * Outdated architectural descriptions
  * Terminology drift
  * Inconsistencies with implementation (once engine is public)

* **Before Major Releases** - Verify:
  * All ADRs are current
  * Architecture documentation matches implementation
  * Safety model reflects actual constraints
  * Examples and specifications are accurate

* **After Significant Architectural Changes** - Update:
  * Affected ADRs (or create new ones)
  * Architecture documentation
  * Canonical Glossary (if terminology changes)
  * Cross-references in related documents

### Review Checklist

When reviewing documentation:

- [ ] Are all cross-references working?
- [ ] Is terminology consistent with the Canonical Glossary?
- [ ] Are ADRs still accurate or do they need status updates?
- [ ] Do examples reflect current architecture?
- [ ] Are non-goals still accurate?
- [ ] Is the tone consistent (precise, calm, professional, opinionated where appropriate)?
- [ ] Are there duplicated explanations that should be consolidated?
- [ ] Are diagrams (text-based or visual) still accurate?

---

## When to Create a New ADR vs. Update an Existing One

### Create a New ADR When:

* **A new architectural decision is made** that affects system structure, behavior, or boundaries
* **A significant constraint is added or removed** (e.g., new safety boundary, capability model change)
* **A major component is introduced** (e.g., new memory layer, new module type)
* **An existing ADR is superseded** (mark the old one as "Superseded by ADR-XXX")

### Update an Existing ADR When:

* **Clarifying existing decisions** without changing the decision itself
* **Adding context or rationale** that was implicit but not written
* **Fixing errors or typos** in the description
* **Adding cross-references** to related documents

### Do NOT Update ADRs For:

* Implementation details (these go in code comments or implementation docs)
* Tactical choices that don't affect architecture
* Temporary workarounds
* Platform-specific configurations

---

## ADR Lifecycle and Status Values

ADRs use these status values:

* **Proposed** - Under discussion, not yet adopted
* **Accepted** - Adopted and guiding implementation
* **Deprecated** - No longer recommended, but not yet replaced
* **Superseded** - Replaced by a newer ADR (include reference: "Superseded by ADR-XXX")
* **Rejected** - Considered but explicitly rejected (keep for historical context)

**Status updates** are appropriate when:
* An architecture decision is no longer valid
* A better approach supersedes an older one
* Implementation reveals the decision was incorrect

**Include a note** explaining why the status changed and linking to the superseding ADR if applicable.

---

## How to Propose Documentation Changes

### For Minor Changes (Typos, Formatting, Broken Links)

1. **Identify the issue** (broken link, typo, formatting inconsistency)
2. **Verify it's not intentional** (check related documents for context)
3. **Submit a pull request** with:
   * Clear description of what was wrong
   * What was changed
   * Why the change is correct

### For Clarifications and Additions

1. **Check for authority duplication** - Is this content already covered elsewhere?
2. **Identify the canonical source** - Should this be added to an existing document or be new?
3. **Follow existing tone** - Precise, calm, professional, opinionated where appropriate
4. **Use canonical terminology** - Reference the [Canonical Glossary](foundations/canonical-glossary.md)
5. **Add cross-references** - Link to related authoritative sources
6. **Submit a pull request** explaining:
   * What gap the change fills
   * Why this is the right place for the content
   * How it maintains consistency with existing docs

### For Architectural or Conceptual Changes

1. **Discuss first** - Architectural changes require consensus
2. **Determine if a new ADR is needed** (see "When to Create a New ADR" above)
3. **Draft the change** maintaining:
   * Consistency with canonical terminology
   * Alignment with existing architectural decisions
   * Clarity and precision
4. **Update cross-references** - Find all documents that reference the changed concept
5. **Submit a pull request** with:
   * Rationale for the change
   * Impact on existing documentation
   * List of affected cross-references

---

## Canonical Glossary Updates

The [Canonical Glossary](foundations/canonical-glossary.md) is **authoritative**. Changes require special care.

### When to Update the Glossary

* A new architectural component is introduced
* An existing definition is ambiguous or incomplete
* Terminology drift is detected
* An ADR introduces new canonical terms

### How to Update the Glossary

1. **Verify the term is architectural** - Tactical or implementation details don't belong here
2. **Check for conflicts** - Does this conflict with existing definitions?
3. **Write the definition precisely** - Be complete but concise
4. **Add cross-references** - Link to related ADRs and architecture docs
5. **Update all usage** - Search documentation for the term and ensure consistent usage
6. **Get review** - Glossary changes affect the entire documentation set

**Never** change an existing canonical definition without:
* Documenting the reason for the change
* Updating all references to that term
* Considering if an ADR status update is needed

---

## Documentation Structure and Organization

### Current Directory Structure

```
/
├── README.md                  # Entry point and high-level overview
├── GETTING_STARTED.md         # Navigation guide for new readers
├── FAQ.md                     # Common questions with concise answers
├── DOCUMENTATION_MAINTENANCE.md # This document
├── adr/                       # Architecture Decision Records (numbered)
├── architecture/              # Detailed architecture documentation
├── foundations/               # Canonical principles and glossary
├── vision/                    # Vision, goals, and non-goals
├── governance/                # Licensing and project viability
├── safety/                    # Safety model, threat model, memory integrity
├── specs/                     # Technical specifications (URN, JSRS)
├── roadmaps/                  # Development roadmap
└── announcements/             # Project announcements
```

### Adding New Documents

**Before adding a new document:**

1. **Check if content belongs in an existing document** - Avoid fragmentation
2. **Determine the appropriate directory** - Follow the existing structure
3. **Ensure it has a clear, unique purpose** - No authority duplication
4. **Use a descriptive filename** - Lowercase, hyphen-separated (e.g., `new-concept.md`)

**When adding a new document:**

1. Include a **purpose statement** at the top
2. Add **cross-references** to related documents
3. Update navigation documents (README, GETTING_STARTED.md, relevant directory READMEs)
4. Use canonical terminology
5. Follow existing formatting conventions

### Removing or Renaming Documents

**Avoid renaming or removing documents.** This breaks external links and disrupts navigation.

**If a document must be renamed:**
* Create a redirect or stub at the old location pointing to the new one
* Update all cross-references
* Document the change in announcements

**If a document must be removed:**
* Ensure content is preserved elsewhere (if still valid)
* Update all cross-references
* Consider leaving a stub explaining where the content moved

---

## Cross-Reference Maintenance

Cross-references are critical for navigation. Broken links undermine documentation quality.

### Best Practices

* **Use relative paths** - `[Canonical Glossary](foundations/canonical-glossary.md)`
* **Link to authoritative sources** - Don't repeat content, link to it
* **Use descriptive link text** - Not "click here", but "see [Seed System](architecture/seed-system.md)"
* **Verify links before committing** - Especially after file moves or renames

### Finding Broken Links

Periodically check for broken links:

```bash
# Find all markdown links
grep -r "\[.*\](.*\.md)" *.md */*.md

# Check if referenced files exist
# (Manual verification or automated link checker)
```

---

## Tone and Style Guidelines

ANIMA documentation uses a **precise, calm, professional, opinionated** tone.

### Do:
* Be precise and unambiguous
* State decisions clearly and confidently
* Explain rationale without being defensive
* Use "she" for ANIMA (when referring to an instance, not the engine)
* Use technical terminology correctly
* Be honest about limitations and constraints

### Don't:
* Use marketing language or hype
* Hedge unnecessarily ("maybe", "perhaps", "might")
* Apologize for design decisions
* Use humor at the expense of clarity
* Soften safety boundaries or constraints
* Introduce new terminology without canonical definitions

### Examples

**Good:**
> "ANIMA is not designed to operate as an autonomous internet agent. This is an explicit boundary chosen to protect safety, not a temporary limitation."

**Bad:**
> "We're not quite ready for internet access yet, but we might add it later if we can make it safe enough."

**Good:**
> "The Cognitive Kernel supervises execution through lease-gated modules. This architectural invariant ensures no action occurs without valid cryptographic authorization."

**Bad:**
> "We use a cool lease system that kind of makes sure stuff is authorized."

---

## Documentation Changelog (Optional)

For major documentation updates, consider maintaining a lightweight changelog:

### Format

```markdown
## [Date] - [Type of Change]

### Added
- New document: [Title](path/to/document.md)
- New section in [Document](path.md): [Section Name]

### Changed
- Updated [Document](path.md): [Description of change]
- Revised ADR-XXX status to [New Status]

### Fixed
- Corrected broken links in [Document](path.md)
- Fixed terminology inconsistency in [Document](path.md)
```

### When to Update

* Major documentation restructuring
* New ADRs published
* Significant content additions
* Terminology changes in Canonical Glossary

---

## Process Summary

### For Documentation Contributors

1. **Read existing documentation first** - Understand the current state
2. **Follow the style guide** - Maintain consistency
3. **Use canonical terminology** - Check the Glossary
4. **Add cross-references** - Link to authoritative sources
5. **Test all links** - Ensure they work
6. **Keep it precise** - Clarity over cleverness
7. **Get review** - Documentation changes affect all readers

### For Documentation Reviewers

1. **Check terminology consistency** - Does it match the Glossary?
2. **Verify cross-references** - Are links correct and appropriate?
3. **Assess tone** - Is it precise, calm, professional, opinionated?
4. **Look for duplication** - Is authority split across multiple documents?
5. **Evaluate structure** - Is this the right place for this content?
6. **Consider impact** - How does this change affect related documents?

---

## Contact and Discussion

For questions about documentation:
* Review existing issues and pull requests
* Propose changes via pull requests
* Include rationale and context in PR descriptions

---

**Last Updated:** January 28, 2026
