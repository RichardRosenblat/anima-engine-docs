# 🧩 ANIMA — REFINED ARCHITECTURE DIAGRAM (ALIGNED)

---

## 🌍 OUTSIDE WORLD

```
[ User ]        [ Platforms / Hardware / APIs ]
```

No intelligence here. Just reality.

---

## 🟦 MODULE LAYER (Effectful Boundary)

> **Only layer that touches the real world**

```
┌───────────────────────────────────────────┐
│               MODULES                     │
│                                           │
│  • Discord Input Module                   │
│  • CLI Input Module                       │
│  • Microphone Input Module                │
│                                           │
│  • Discord Output Module                  │
│  • CLI Output Module                      │
│  • TTS Output Module                      │
│  • Live2D Output Module                   │
│                                           │
│  (APIs, hardware, streaming, devices)     │
└───────────────────────────────────────────┘
```

📌 Rule:

* Modules **capture** or **execute**
* Modules **do not think**
* Modules **do not decide**

---

## 🟨 ADAPTER LAYER (Pure Translation Ring)

> **First protective ring around the core**

```
┌───────────────────────────────────────────┐
│               ADAPTERS                    │
│                                           │
│  Input Adapters:                          │
│   • Discord → CoreInput                   │
│   • CLI → CoreInput                       │
│   • Mic → CoreInput                       │
│                                           │
│  Output Adapters:                         │
│   • Intent → DiscordCommand               │
│   • Intent → TTSCommand                   │
│   • Intent → Live2DCommand                │
│                                           │
│  (Pure, deterministic, no I/O)            │
└───────────────────────────────────────────┘
```

📌 Rule:

* Adapters **translate only**
* No side effects
* No memory
* No permissions


---

## 🟩 CAPABILITY RING (Declarative Power)

> **What the core is allowed to want**

```
┌───────────────────────────────────────────┐
│              CAPABILITIES                 │
│                                           │
│  • send_text                              │
│  • speak_audio                            │
│  • render_avatar                         │
│  • move_robot                            │
│                                           │
│  (Contracts, not implementations)         │
└───────────────────────────────────────────┘
```

📌 Rule:

* Capabilities are symbolic
* Seed + Security gate them
* They do not execute anything

---

## 🧠 CORE (Reasoning Engine)

> **The only place where decisions are made**

```
┌───────────────────────────────────────────┐
│                   CORE                    │
│                                           │
│  • Reasoning Loop                         │
│  • Intent Planning                        │
│  • Task Management                        │
│  • Capability Selection                  │
│                                           │
│  Inputs:                                  │
│   • CoreInput                             │
│   • Memory Query Results                  │
│   • Seed Constraints                     │
│   • Permissions                          │
│                                           │
│  Output:                                  │
│   • Intent Graph / Plan                  │
└───────────────────────────────────────────┘
```

📌 Rule:

* Core **never touches the world**
* Core produces **intent**, not effects

---

## 🟦 INTERNAL CONTEXT (Influence, Not Control)

These surround the core but **do not execute**.

### 🧬 Seed (Static Identity)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Personality parameters │
│  • Tone / expressiveness  │
│  • Risk tolerance         │
│  • Allowed capabilities   │
│  • Identity boundaries    │
└───────────────────────────┘
```

* Loaded at startup
* Immutable during runtime

---

### 🧠 Memory (Dynamic, Fallible)

```
┌───────────────────────────┐
│          MEMORY           │
│                           │
│  • Past interactions      │
│  • Observations           │
│  • Task states            │
│  • Confidence-weighted    │
│    facts                  │
└───────────────────────────┘
```

* Instance-local
* Queried, never blindly trusted

---

## 🔐 SECURITY & POLICY (Cross-Cutting)

```
┌───────────────────────────┐
│          SECURITY         │
│                           │
│  • Authentication         │
│  • Authorization          │
│  • Permission enforcement │
│  • Dangerous action gates │
└───────────────────────────┘
```

Security:

* wraps **input before core**
* validates **intent before execution**

---

## 🔁 FULL FLOW (CLEAN & LINEAR)

```
User
 ↓
Input Module
 ↓
Input Adapter
 ↓
Authentication / Security
 ↓
CORE
  ↔ Memory
  ↔ Seed
  ↔ Capabilities
 ↓
Intent
 ↓
Output Adapter
 ↓
Output Module
 ↓
Effect
```

No shortcuts. No leaks.

---

## 🧠 Architectural Litmus Test 
Ask:

* Can I simulate everything without modules? → Yes
* Can I swap Discord for Slack without touching the core? → Yes
* Can I run multiple Seeds on the same engine? → Yes
* Can I audit intent before execution? → Yes
