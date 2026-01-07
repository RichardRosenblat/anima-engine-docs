# 🧩 Arquitectura ANIMA — Visión General Completa

**Documentación Relacionada:**
* [Sistema de Seeds](seed-system.md) — Inicialización e separação de identidade
* [Tipos de Módulos y Leases](module-types-and-leases.md) — Ciclo de vida e autorização de módulos
* [Arquitectura de Eventos](event-architecture.md) — Sistema de observabilidade e entrada
* [Kernel Cognitivo](cognitive-kernel.md) — Core como supervisor multitarefa
* [Dominio e Infraestructura](domain-and-infrastructure.md) — Implementación de arquitetura hexagonal
* [Topologia de Modelos de IA](ai-model-topology.md) — Cortex e Arcuate
* [División Adapter-Actuator](adapter-actuator-split.md) — Estrutura de módulos

**ADRs Relacionados:** Todos os ADRs (ADR-001 até ADR-011)

---

## Introducción

ANIMA é um **motor de IA privado e modular** diseñado para alojar identidades artificiais de larga duración e em evolução sob estrictas restricciones de seguridad, memória e capacidade.

A arquitetura sigue principios de **Arquitetura Hexagonal** e **Diseño Orientado por Dominio (DDD)**, com forte ênfase em:

* Separación de responsabilidades
* Seguridad por design
* Comportamento observable e auditable
* Aislamiento de identidade
* Extensibilidad modular

---

## Principios Arquitectónicos Centrales

### 1. Motor ≠ Identidade

O Motor ANIMA é agnóstico de identidade. Personalidade e comportamento se introducen a través de **Seeds** (vea [Sistema de Seeds](seed-system.md)).

* Motor: razonamiento, planificación, mecánica de memoria, seguridad
* Identidade: personalidade, tom, políticas de comportamiento (definidas por Seed)

### 2. Arquitetura Hexagonal

> **Los dominios nunca deben falar diretamente com o mundo externo.**

* Domínios definem **portas** (interfaces)
* Infraestrutura fornece **adaptadores** (implementaciones)
* Runtime compone el sistema

Veja [Dominio e Infraestructura](domain-and-infrastructure.md) para detalles.

### 3. Observabilidade e Entrada Baseadas em Eventos

* Toda observabilidade é expresa como **eventos estructurados**
* Todas as entradas são transformadas em **eventos** antes de llegar al Core
* Sin logs tradicionales, solo hechos inmutables

Veja [Arquitectura de Eventos](event-architecture.md) para detalles.

### 4. Core como Kernel Cognitivo

O Core ANIMA se comporta como um **Kernel Cognitivo**, supervisando tareas concurrentes en lugar de ejecutarlas directamente.

Veja [Kernel Cognitivo](cognitive-kernel.md) para detalles.

### 5. Autorización Baseada em Lease

Toda comunicação Core ↔ Módulo é controlada por **leases criptográficos** sobre **gRPC com mTLS**.

Veja [Tipos de Módulos y Leases](module-types-and-leases.md) para detalles.

---

## 🌍 MUNDO EXTERNO

```
[ Usuario ]        [ Plataformas / Hardware / APIs ]
```

Sin inteligencia aquí. Solo realidad.

---

## 🟦 CAPA DE MÓDULOS (Frontera con Efectos)

> **Única capa que toca el mundo real**

```
┌───────────────────────────────────────────┐
│               MÓDULOS                     │
│                                           │
│  • Módulo de Entrada Discord              │
│  • Módulo de Entrada CLI                  │
│  • Módulo de Entrada Micrófono            │
│                                           │
│  • Módulo de Salida Discord                │
│  • Módulo de Salida CLI                    │
│  • Módulo de Salida TTS                    │
│  • Módulo de Salida Live2D                 │
│                                           │
│  (APIs, hardware, streaming, dispositivos)│
└───────────────────────────────────────────┘
```

📌 **Reglas de Módulos:**

* Módulos **capturam** ou **executam**
* Módulos **no piensan**
* Módulos **no deciden**
* Todos os módulos executam **fora de processo** do Core
* Comunicación a través de **gRPC com mTLS**

### Tipos de Módulos

Cada módulo declara uno de tres tipos:

* **Tipo I** — Privado Efímero (vinculado a lease, Core único)
* **Tipo II** — Privado Residente (larga duración, Core único)
* **Tipo III** — Compartilhado Residente (multi-tenant, gobernado por infraestructura)

Veja [Tipos de Módulos y Leases](module-types-and-leases.md) para especificaciones completas.

---

## 🟨 CAPA DE ADAPTADORES (Anillo de Traducción Pura)

> **Primer anillo protector em torno do core**

```
┌───────────────────────────────────────────┐
│               ADAPTADORES                 │
│                                           │
│  Adaptadores de Entrada:                  │
│   • Eventos de plataforma → input.nl      │
│   • Señales de sistema → input.system      │
│   • Módulo NLP → input.semantic           │
│                                           │
│  Adaptadores de Salida:                    │
│   • Intención → Comandos de Módulo         │
│   • Validación de schema                   │
│   • Imposición de lease                    │
│                                           │
│  (Puro, determinístico, sem I/O)          │
└───────────────────────────────────────────┘
```

📌 **Reglas de Adaptadores:**

* Adaptadores **solo traducen**
* Sem efectos secundarios
* Sem acceso a memoria
* Sem permisos
* Executam **fora de processo** do Core
* Validan contra **JSON Schemas**

Veja [División Adapter-Actuator](adapter-actuator-split.md) para detalles.

---

## 🔄 CAPA DE ACTUADORES (Ejecución)

> **Ejecución com efeitos sob controle rigoroso de lease**

Actuadores:

* Executam efectos secundarios do mundo real (APIs, hardware, plataformas)
* Imponen alcance de lease e políticas de interrupção
* Emiten eventos de observabilidad
* Contienen cero lógica del Core

Veja [División Adapter-Actuator](adapter-actuator-split.md) para detalles.

---

## 🟩 ANILLO DE CAPACIDADES (Poder Declarativo)

> **Lo que el core tiene permiso para querer**

```
┌───────────────────────────────────────────┐
│              CAPACIDADES                  │
│                                           │
│  • send_text                              │
│  • speak_audio                            │
│  • render_avatar                          │
│  • move_robot                             │
│                                           │
│  (Contratos, não implementaciones)          │
└───────────────────────────────────────────┘
```

📌 **Reglas de Capacidades:**

* Capacidades son contratos simbólicos
* Seed + Seguridad los controlan
* No ejecutan nada directamente
* Declarados vía **JSON Schemas**
* Versionados e inmutables

---

## 🧠 CORE (Kernel Cognitivo)

> **El único lugar donde se toman decisiones**

```
┌───────────────────────────────────────────┐
│            KERNEL COGNITIVO               │
│                                           │
│  🧩 Cortex (Obligatorio)                  │
│    • Planificación & Razonamiento            │
│    • Expansión de Tareas                  │
│    • Selección de Capacidades               │
│                                           │
│  💬 Arcuate (Opcional)                    │
│    • Procesamiento NLP                    │
│    • input.nl → input.semantic            │
│    • Intenção → Lenguaje Natural         │
│                                           │
│  🎯 Supervisor de Tareas                 │
│    • Gestión de tareas concurrentes│
│    • Enrutamiento de interrupciones           │
│    • Ciclo de vida de spans               │
│                                           │
│  Entradas:                                │
│   • Eventos Estruturados                  │
│   • Resultados de Consulta de Memoria     │
│   • Restricciones de Seed                    │
│   • Políticas de Seguridad                │
│                                           │
│  Salida:                                   │
│   • Grafos de Intención                    │
│   • Directivas de Tareas                  │
│   • Eventos de Observabilidad            │
└───────────────────────────────────────────┘
```

📌 **Reglas del Core:**

* Core **nunca toca el mundo**
* Core produz **intenção**, não efeitos
* Core **supervisiona** trabalho, não o executa
* Todas as ações são **interruptíveis por design**
* **Cortex** (cognição) é obrigatório
* **Arcuate** (NLP) é opcional

Veja [Kernel Cognitivo](cognitive-kernel.md) e [Topologia de Modelos de IA](ai-model-topology.md) para detalles.

---

## 🟦 CONTEXTO INTERNO (Influencia, No Control)

Estes envolvem o core mas **não executam**.

### 🧬 Seed (Identidad Estática)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Parâmetros de          │
│    personalidade          │
│  • Tom / expresividad   │
│  • Tolerancia a riesgo     │
│  • Capacidades permitidas │
│  • Límites de identidad  │
│  • Políticas de memoria   │
└───────────────────────────┘
```

* Cargado en la inicialización
* Inmutable durante ejecución
* Datos, no código
* Define instancia pero no es único a la instancia

Veja [Sistema de Seeds](seed-system.md) para detalles.

---

### 🧠 Memoria (Dinámica, Falible)

```
┌───────────────────────────┐
│          MEMÓRIA          │
│                           │
│  • Interacciones pasadas    │
│  • Observaciones            │
│  • Estados de tareas     │
│  • Fatos ponderados por   │
│    confiança              │
│  • Apenas local à         │
│    instância              │
└───────────────────────────┘
```

* Local a la instancia
* Consultada, nunca confiada ciegamente
* En capas (episódica, semántica, narrativa)
* Sin compartir entre instancias

---

## 🔐 SEGURANÇA & POLÍTICA (Transversal)

```
┌───────────────────────────┐
│         SEGURANÇA         │
│                           │
│  • Autenticación           │
│  • Autorización            │
│  • Gestión de Lease │
│  • Imposición de permisos│
│  • Controles de ações     │
│    perigosas              │
│  • Registro de auditoría  │
└───────────────────────────┘
```

Seguridad:

* Envolve **entrada antes do core**
* Valida **intenção antes da execução**
* Emite e gerencia **leases criptográficos**
* Impõe **limites de capacidade**

---

## 🔁 FLUJO COMPLETO (LIMPIO & LINEAL)

```
Usuario / Ambiente
 ↓
Módulo de Entrada (captura)
 ↓
Adaptador (traduce a eventos)
 ↓
Eventos: input.nl / input.system / input.semantic
 ↓
Autenticación / Seguridad
 ↓
KERNEL COGNITIVO
  ↔ Memória (consulta)
  ↔ Seed (restricciones)
  ↔ Cortex (razonamiento)
  ↔ Arcuate (NLP opcional)
 ↓
Intenção + URN de Capacidade
 ↓
Seguridad (valida lease & permisos)
 ↓
Adaptador de Salida (valida schema, mapea a comando)
 ↓
Atuador (ejecuta bajo lease)
 ↓
Efeito + Eventos de Observabilidad
```

Sin atajos. Sin fugas. Todos os pasos observables y auditables.

---

## Arquitectura Basada en Eventos

Toda actividad del sistema se expresa através de **eventos estructurados**:

* **input.nl** — Lenguaje natural (pre-semántica)
* **input.system** — Hechos de sistema/mecánicos
* **input.semantic** — Significado afirmado post-interpretación
* **capability.invoked** — Ejecución de capacidade iniciada
* **capability.completed** — Ejecución de capacidade finalizada
* **capability.interrupted** — Ejecución de capacidade interrompida

Los eventos son:
* Inmutables
* Con alcance de ejecución
* Rastreables causalmente
* Seguros para concurrencia

Veja [Arquitectura de Eventos](event-architecture.md) para detalles.

---

## Interrupción & Multitarea

O Core opera como um **kernel multitarefa** que suporta:

* Ejecución concorrente de tarefas
* Interrupción cooperativa
* Gestión explícita de spans
* Preempción gobernada por política

Las interrupciones son:
* Eventos de primera clase
* Clasificadas (override/cancel/queue/clarification/emergency)
* Gobernadas por política
* Observables y auditables

Veja [Kernel Cognitivo](cognitive-kernel.md) para detalles.

---

## Diseño Orientado por Dominio

ANIMA segue regras rigorosas de **Arquitetura Hexagonal**:

```
infra  ─────▶  domínios
runtime ────▶  infra + domínios

domínios ─╳──▶  infra
```

* Domínios definem **portas** (interfaces)
* Infraestrutura implementa **adaptadores**
* Domínios contêm **apenas lógica de negócio**
* Runtime ensambla el sistema

Veja [Dominio e Infraestructura](domain-and-infrastructure.md) para detalles.

---

## 🧠 Prueba de Litmus Arquitectónica

Pregunte:

* Puedo simular todo sin módulos? → **Sim**
* Puedo cambiar Discord por Slack sin tocar el core? → **Sim**
* Puedo ejecutar múltiples Seeds en el mismo motor? → **Sim**
* Puedo auditar intención antes de la ejecución? → **Sim**
* Las interrupciones pueden ser reproducidas de logs de eventos? → **Sim**
* Los módulos pueden ejecutar sin código del Core cargado? → **Sim**
* Toda execução é rastreável e observable? → **Sim**

---

## Invariantes Arquitectónicas Principales

Os seguintes são **restricciones arquiteturais não-negociáveis**:

1. **Motor é agnóstico de identidade** — personalidade vem de Seeds
2. **Core nunca carrega código de terceiros** — módulos executam fora de processo
3. **Toda execução requer leases válidos** — sem lease, sem execução
4. **Domínios nunca importam infraestrutura** — apenas portas/interfaces
5. **Toda observabilidade é baseada em eventos** — sem logs tradicionais
6. **Memória é estritamente com escopo de instância** — sem compartilhamento entre instâncias
7. **Cortex é obrigatório, Arcuate é opcional** — cognição vs. linguagem
8. **Core supervisiona, não executa** — modelo de kernel
9. **Todas las acciones son interrumpibles** — interrupção cooperativa
10. **Los eventos son a fonte da verdade** — fatos imutáveis e auditáveis

---

## Resumen

ANIMA é um **kernel cognitivo** que:

* Separa motor de identidad
* Supervisa comportamiento en lugar de ejecutarlo
* Comunica através de eventos estructurados
* Impõe seguridad através de leases criptográficos
* Mantiene límites arquitectónicos limpios
* Habilita identidades de larga duración e em evolução

> **ANIMA no ejecuta instrucciones. Supervisa comportamiento.**
> **ANIMA no registra texto. Registra hechos.**
> **ANIMA es un motor para identidades en crecimiento, no una personalidad única.**
