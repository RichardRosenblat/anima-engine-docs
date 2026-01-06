# 🧩 ANIMA — DIAGRAMA DE ARQUITECTURA REFINADO (ALINEADO)

---

## 🌍 MUNDO EXTERNO

```
[ Usuario ]        [ Plataformas / Hardware / APIs ]
```

No hay inteligencia aquí. Solo realidad.

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
│  • Módulo de Salida Discord               │
│  • Módulo de Salida CLI                   │
│  • Módulo de Salida TTS                   │
│  • Módulo de Salida Live2D                │
│                                           │
│  (APIs, hardware, streaming, dispositivos)│
└───────────────────────────────────────────┘
```

📌 Regla:

* Los módulos **capturan** o **ejecutan**
* Los módulos **no piensan**
* Los módulos **no deciden**

---

## 🟨 CAPA DE ADAPTADORES (Anillo de Traducción Pura)

> **Primer anillo protector alrededor del núcleo**

```
┌───────────────────────────────────────────┐
│               ADAPTADORES                 │
│                                           │
│  Adaptadores de Entrada:                  │
│   • Discord → EntradaNúcleo               │
│   • CLI → EntradaNúcleo                   │
│   • Mic → EntradaNúcleo                   │
│                                           │
│  Adaptadores de Salida:                   │
│   • Intención → ComandoDiscord            │
│   • Intención → ComandoTTS                │
│   • Intención → ComandoLive2D             │
│                                           │
│  (Puro, determinista, sin I/O)            │
└───────────────────────────────────────────┘
```

📌 Regla:

* Los adaptadores **solo traducen**
* Sin efectos secundarios
* Sin memoria
* Sin permisos


---

## 🟩 ANILLO DE CAPACIDADES (Poder Declarativo)

> **Lo que el núcleo puede desear**

```
┌───────────────────────────────────────────┐
│              CAPACIDADES                  │
│                                           │
│  • enviar_texto                           │
│  • hablar_audio                           │
│  • renderizar_avatar                      │
│  • mover_robot                            │
│                                           │
│  (Contratos, no implementaciones)         │
└───────────────────────────────────────────┘
```

📌 Regla:

* Las capacidades son simbólicas
* Seed + Seguridad las controlan
* No ejecutan nada

---

## 🧠 NÚCLEO (Motor de Razonamiento)

> **El único lugar donde se toman decisiones**

```
┌───────────────────────────────────────────┐
│                   NÚCLEO                  │
│                                           │
│  • Bucle de Razonamiento                  │
│  • Planificación de Intención             │
│  • Gestión de Tareas                      │
│  • Selección de Capacidades               │
│                                           │
│  Entradas:                                │
│   • EntradaNúcleo                         │
│   • Resultados de Consulta de Memoria    │
│   • Restricciones de Seed                 │
│   • Permisos                              │
│                                           │
│  Salida:                                  │
│   • Grafo de Intención / Plan             │
└───────────────────────────────────────────┘
```

📌 Regla:

* El núcleo **nunca toca el mundo**
* El núcleo produce **intención**, no efectos

---

## 🟦 CONTEXTO INTERNO (Influencia, No Control)

Estos rodean el núcleo pero **no ejecutan**.

### 🧬 Seed (Identidad Estática)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Parámetros de          │
│    personalidad           │
│  • Tono / expresividad    │
│  • Tolerancia al riesgo   │
│  • Capacidades permitidas │
│  • Límites de identidad   │
└───────────────────────────┘
```

* Cargada en el inicio
* Inmutable durante ejecución

---

### 🧠 Memoria (Dinámica, Falible)

```
┌───────────────────────────┐
│          MEMORIA          │
│                           │
│  • Interacciones pasadas  │
│  • Observaciones          │
│  • Estados de tareas      │
│  • Hechos ponderados por  │
│    confianza              │
└───────────────────────────┘
```

* Local a la instancia
* Consultada, nunca confiada ciegamente

---

## 🔐 SEGURIDAD Y POLÍTICA (Transversal)

```
┌───────────────────────────┐
│          SEGURIDAD        │
│                           │
│  • Autenticación          │
│  • Autorización           │
│  • Aplicación de permiso  │
│  • Puertas de acción      │
│    peligrosa              │
└───────────────────────────┘
```

Seguridad:

* envuelve **entrada antes del núcleo**
* valida **intención antes de la ejecución**

---

## 🔁 FLUJO COMPLETO (LIMPIO Y LINEAL)

```
Usuario
 ↓
Módulo de Entrada
 ↓
Adaptador de Entrada
 ↓
Autenticación / Seguridad
 ↓
NÚCLEO
  ↔ Memoria
  ↔ Seed
  ↔ Capacidades
 ↓
Intención
 ↓
Adaptador de Salida
 ↓
Módulo de Salida
 ↓
Efecto
```

Sin atajos. Sin fugas.

---

## 🧠 Prueba Litmus Arquitectural
Pregunta:

* ¿Puedo simular todo sin módulos? → Sí
* ¿Puedo cambiar Discord por Slack sin tocar el núcleo? → Sí
* ¿Puedo ejecutar múltiples Seeds en el mismo motor? → Sí
* ¿Puedo auditar intención antes de la ejecución? → Sí
