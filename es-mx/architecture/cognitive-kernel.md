# Cognitive Kernel — ANIMA Core as a Multitasking Supervisor

**ADRs Relacionados:**
* [ADR-005: Interruption & Preemption Model](../adr/ADR-005.md)
* [ADR-008: ANIMA Core Behaves as a Cognitive Kernel with Multitasking and Interruptible Execution](../adr/ADR-008.md)

---

## Visión General

ANIMA is not a request–response chatbot. It is a long-lived, stateful cognitive system that operates more like an **operating system kernel** than a traditional application.

The ANIMA Core is formally defined as a **Cognitive Kernel**.

---

## What is a Cognitive Kernel?

The ANIMA Core, acting as a kernel, is responsible for:

* Task lifecycle management (spawn, run, pause, cancel, complete)
* Interrupt routing and arbitration
* Event dispatch and prioritization
* Capability-aware execution control
* Resource-aware scheduling
* Observability and audit logging
* Safe interaction between cognition, language, and embodiment

The Core does not "do work" directly. It **supervises work**.

---

## Tasks as Managed Execution Units

A task in ANIMA is an execution of a plan that contains multiple action intent objects and contingencies.

Tasks are:

* Explicitly created
* Identifiable
* Observable
* Interruptible
* Cooperative in their execution

Tasks MUST:

* Expose lifecycle state
* Periodically check for control signals
* Terminate cleanly when interrupted
* Never assume uninterrupted execution

This mirrors operating system process semantics, not traditional async calls.

---

## Cooperative Interruption Model

ANIMA adopts **cooperative interruption**, not forceful termination.

The Kernel MAY:

* Signal pause
* Signal cancellation
* Signal priority change

Tasks MUST decide:

* When it is safe to pause
* How to clean up resources
* How to terminate gracefully

This guarantees:

* Resource safety
* Predictable behavior
* Explainable state transitions

---

## Interruptions Are First-Class Events

Interruptions are **not exceptions**, signals, or side-channels.

All interruptions MUST enter the system as **structured input events**, processed by the Core like any other input.

Examples include:

* User speech while ANIMA is speaking
* User commands issued mid-execution
* System safety triggers
* Capability-emitted alerts

Interruptions MUST be:

* Observable
* Auditable
* Replayable
* Attributable to a source

---

## Execution Model: Spans & Focus

All Core activity is represented as **execution spans**, each with:

* A unique `span_id`
* An associated `execution_id`
* A declared `interruptible` flag
* An interruption policy

At any moment, the Core maintains a **foreground focus span**.

By default:

* Only the foreground span may be interrupted
* Background work continues unless explicitly escalated

This prevents uncontrolled cascade cancellation.

---

## Interruptibility Is Explicit

No action is interruptible by default.

Only spans that explicitly declare:

```yaml
interruptible: true
```

may be interrupted.

This includes, but is not limited to:

* Speech streaming
* Listening
* Movement
* Waiting
* Long-lived capability execution

Non-interruptible spans MUST ignore interruption events.

---

## Interruption Classification

Upon receiving an interruption event, the Core MUST classify it before acting.

Supported interruption classes include:

| Class           | Semantics                               |
| --------------- | --------------------------------------- |
| `override`      | Replace current foreground execution    |
| `cancel`        | Stop the current execution              |
| `queue`         | Defer until current execution completes |
| `clarification` | Interrupt to request clarification      |
| `emergency`     | Immediate halt, bypassing normal flow   |

Classification is a **reasoning step**, not a reflex.

---

## Capability Interruption Policies

Capabilities that expose interruptible methods MUST declare an **interruption policy** per method.

Supported policies include:

| Policy              | Behavior                                  |
| ------------------- | ----------------------------------------- |
| `soft-stop`         | Stop at a safe boundary (e.g. buffer end) |
| `hard-stop`         | Immediate termination                     |
| `checkpointed`      | Roll back to last known safe checkpoint   |
| `non-interruptible` | Reject all interruptions                  |

The Core MUST respect declared policies. Capabilities MUST enforce them locally.

---

## Interruption Handling Flow

When an interruption occurs:

1. An interruption input event is emitted
2. The Core evaluates:
   * Current foreground span
   * Interruptibility
   * Interruption classification
   * Security & confidence thresholds
3. If permitted:
   * The target span is marked `interrupted`
   * An interruption directive is sent to the capability (if applicable)
4. The interrupted span:
   * Terminates according to its policy
   * Emits completion or cancellation events
5. A **new execution trace** begins

Interrupted traces are **never mutated or deleted**.

---

## Speaking While Speaking (Canonical Case)

If ANIMA is actively speaking and user speech is detected:

* Speech detection enters as an interruption event
* The Core evaluates confidence and permissions
* If classified as `override` or `cancel`:
  * The speech span is interrupted
  * The voice capability performs a `soft-stop`
  * Control returns to the Core
  * A new execution begins

No hard process termination is permitted.

---

## Kernel vs User Space

The Core enforces a strict separation:

### Kernel Space

* Task scheduling
* Memory access mediation
* Capability enforcement
* AI model invocation
* Interrupt routing

### User Space (Modules / Tasks)

* Domain-specific logic
* IO and side effects (via capabilities)
* Cooperative task execution

Modules MUST NOT:

* Manage concurrency independently
* Interrupt other tasks directly
* Access kernel internals
* Bypass supervision

---

## Modelo de Multitarea

The Kernel MUST support concurrent execution of:

* Cognitive planning
* Natural language processing
* Listening / sensing tasks
* Speaking / embodiment tasks
* Monitoring or background behaviors

Multitasking is **first-class**, not incidental.

The Kernel remains responsive even while tasks are running.

---

## Security & Abuse Mitigation

Interruptions may be malicious or accidental.

The Core MUST enforce:

* Confidence thresholds (e.g. speech certainty)
* Rate limits
* Role-based permissions
* Escalation rules for emergency interruptions

Examples:

* Only privileged roles may issue `emergency` interruptions
* Low-confidence inputs may be queued or ignored

---

## Observability & Traceability

All interruptions MUST generate timeline events, including:

* Interruption source
* Target span
* Classification
* Policy applied
* Outcome

This integrates directly with the event-based observability model defined in [Event Architecture](event-architecture.md).

Interruptions MUST be clearly visible in execution timelines.

---

## Restricciones and Invariants

The following constraints are architectural invariants:

* The Core MUST never block on task execution
* All long-lived behavior MUST be task-based
* All interruptions MUST pass through the Kernel
* Tasks MUST be interruptible by design
* State transitions MUST be observable and logged
* No module may unilaterally control execution flow

---

## Beneficios

* Enables natural, human-like interaction
* Supports real-time responsiveness
* Makes interruption safe and explainable
* Allows complex behavior composition
* Simplifies reasoning about system state
* Aligns cognition, language, and embodiment under one supervisor
* Full auditability and replayability
* Clean separation between input, reasoning, and execution

---

## Resumen

> **ANIMA does not execute instructions. It supervises behavior.**

> **Interruptions in ANIMA are structured, classified input events that preempt execution in a controlled, observable, and deterministic manner.**

They are:

* Explicit
* Policy-governed
* Auditable
* Safe by construction

This model enables real-time interaction without sacrificing architectural integrity.
