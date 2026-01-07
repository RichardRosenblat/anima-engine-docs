# Adapter–Actuator Split (Module-Owned, Out-of-Process)

This document specifies how Modules are structured to protect the Core from external code while preserving clean contracts.

## Structure

Module package example:
- adapter/ (pure mapping and schema validation)
- actuator/ (effectful execution; APIs, hardware)
- capability_contract/ (JSON Schemas + manifest; versioned)
- transport/ (gRPC server; mTLS; lease enforcement)
- proto/ (IDL only; codegen used inside module process)

## Inbound (Inputs → Core)

- Module runtime captures signals (text, audio, platform events).
- Adapter maps signals to input events with ADR-004 envelope and publishes via gRPC.
- Core ingests events (never raw IO).

## Outbound (Core Intent → Module Command)

- Core produces an Intent with capability URN + payload.
- Core calls the capability gateway on the Module Adapter.
- Adapter validates payload against capability JSON Schema, maps to Actuator command, enforces lease scope, and invokes Actuator.
- Actuator executes and emits events.

## Discovery & Trust

- Modules present a manifest with:
  - module URN
  - capability URNs and versions
  - JSON Schema hashes
  - declared Module Type (ADR-003: I/II/III)
- Core validates manifest over mTLS at handshake; no code loaded from Module.
- All requests carry LeaseID/Epoch; stale epochs are rejected.

## Observability

- Both Adapter and Actuator emit ADR-004 events:
  - capability.invoked
  - capability.completed
  - capability.interrupted
  - validation.failed
- All events include execution_id, trace_id, span_id, and source=module:<name>.

## Why This Is Safe

- Core processes events only, never third-party code.
- Translation and execution are out-of-process.
- Declarative schemas prevent ad-hoc payloads.
- Leases and epochs prevent replay/escalation. 