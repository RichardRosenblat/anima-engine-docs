# 🧭 ANIMA — FASE 0: FUNDAMENTOS

---

## 🎯 Objetivo

Establecer **ANIMA como un motor de identidad**, no una personalidad específica.

ANIMA es:

* un **motor de IA privado**
* capaz de **identidad duradera**
* extensible a través de **capacidades y módulos**
* moldeada en tiempo de ejecución por un **archivo Seed**

ANIMA **no** es:

* un chatbot
* un agente autónomo que se auto-expande
* una plataforma de automatización
* una IA monolítica con comportamiento codificado
* un sistema de rastreo de internet por defecto

---

## 🧱 Construcción (Entregables de la Fase 0)

### 1️⃣ Carta del Proyecto

Este documento responde *"qué existe"* y *"qué está prohibido"*.

#### Propósito Central

* Proporcionar un **runtime de IA privado y evolutivo**
* Separar **motor** de **identidad**
* Soportar **múltiples encarnaciones** vía Seeds
* Permitir **interacción segura y auditable con el mundo**

#### No-Objetivos (Explícitos)

* Sin código auto-modificable
* Sin autonomía descontrolada
* Sin acceso a internet a menos que se conceda vía capacidad
* Sin memoria compartida entre instancias
* Sin permisos implícitos del usuario

#### Valores Centrales

* **Verdad sobre confianza**  
  *¿Qué significa esto?* El sistema prioriza la precisión y honestidad en sus respuestas, incluso si eso significa admitir incertidumbre o falta de conocimiento.
* **Intención sobre ejecución**  
  *¿Qué significa esto?* El sistema se enfoca en comprender y cumplir las intenciones del usuario en lugar de solo ejecutar comandos ciegamente.
* **Modularidad sobre monolito**  
  *¿Qué significa esto?* El sistema está diseñado con componentes intercambiables, permitiendo flexibilidad y adaptabilidad en lugar de ser una entidad única e inmutable.
* **Seguridad sobre capacidad**  
  *¿Qué significa esto?* El sistema prioriza la seguridad del usuario y consideraciones éticas por encima de expandir sus funcionalidades o capacidades.
* **Configurabilidad sobre codificación fija**  
  *¿Qué significa esto?* El sistema enfatiza la capacidad de ser personalizado y configurado a través de ajustes externos en lugar de tener comportamientos fijos y codificados.
* **Aislamiento sobre conveniencia**
  *¿Qué significa esto?* El sistema valora mantener componentes y procesos separados para mejorar la seguridad y confiabilidad, incluso si sacrifica algo de facilidad de uso.

Esta carta es su *ley constitucional*.
Cada característica debe ser justificable contra ella.

---

### 2️⃣ Glosario Canónico

Estas definiciones **nunca deben desviarse**.

#### Motor (Engine)

La totalidad del sistema ANIMA, que contiene:

* Núcleo (Core)
* Semilla (Seed)
* Memoria
* Capacidades
* Módulos
* Adaptadores
* Córtex

---

#### Núcleo (Core)

El bucle de razonamiento dentro del motor.

Él:

* consume entrada
* consulta memoria
* aplica restricciones de Seed
* selecciona capacidades
* produce **intención**

---

#### Semilla/Seed (Archivo)

Un **artefacto de configuración estático** cargado en la inicialización.

Define:

* parámetros de personalidad
* restricciones de comportamiento
* rangos de tono y expresividad
* capacidades permitidas
* límites de identidad
* tolerancia al riesgo

Una Seed:

* **no** contiene memorias
* **no** se modifica
* **no** contiene código

---

#### Memoria

Datos locales de la instancia que describen:

* interacciones pasadas
* observaciones
* estados de tareas
* hechos ponderados por confianza

La Memoria:

* informa el razonamiento
* nunca anula la política
* es falible y consultable

---

#### Capacidad

Un **contrato declarativo** que describe *lo que el núcleo puede intentar*.

Ejemplo:

* `enviar_mensaje`
* `mover_robot`
* `iniciar_stream`

Capacidades:

* no contienen lógica
* no contienen I/O
* están controladas por permiso
* están restringidas por la Seed

---

#### Módulo

Una **implementación con efectos** de una capacidad.

Módulos:

* ejecutan acciones en el mundo real
* hablan con APIs, hardware, plataformas
* nunca deciden *cuándo* o *por qué*
* solo ejecutan *lo que se les dice*

Los módulos son el **único** lugar donde **se detecta la Causa** y **se producen Efectos**.

---

#### Adaptador

Una **capa de traducción pura** entre representaciones.

Adaptadores:

* transforman entrada externa → entrada del núcleo
* transforman intención del núcleo → comando del módulo
* no contienen I/O externo
* contienen solo lógica de traducción
* son determinísticos

Los adaptadores existen para **proteger el núcleo de la contaminación de formato**.

---

#### Intención (Intent)

Una descripción estructurada de **qué debe suceder**, no cómo.

Producida por el núcleo.  
Consumida por adaptadores y módulos.  
Auditable, registrable, reproducible.  
Contiene qué + cuándo + dónde + cuánto + por qué + qué hacer si algo sale mal. Junto con puntuaciones de confianza.

---

#### Tarea (Task)

Una unidad de trabajo de larga duración que el motor lleva a cabo. Resuelta con una serie de Intenciones.

Tareas:

* persisten a lo largo del tiempo
* pueden pausar / reanudar
* pueden invocar capacidades repetidamente
* son rastreadas en memoria

---

#### Córtex (Cortex)

El envoltorio alrededor de un modelo de IA dado, conectado al motor para razonamiento.

Córtices:
* proporcionan servicios de conclusión
* son intercambiables sin necesidad de cambiar el motor
* son reemplazables

---

#### Paquete (Package)

Un grupo distribuible de módulos, adaptadores y definiciones de capacidad.
Puede instalarse en una instancia ANIMA para extender la funcionalidad en masa.

Paquetes:

* agrupan capacidades relacionadas
* incluyen adaptadores para esas capacidades
* están versionados
* pueden compartirse

---

#### Columna Semántica (Semantic Spine)

Una estructura de datos explícita para la representación semántica de un mensaje que se espera pasar al usuario o recibir del usuario.

Las Columnas Semánticas se utilizan para garantizar una comunicación consistente y significativa entre el motor y los usuarios, proporcionando una forma estandarizada de representar el significado y contexto de los mensajes.

Columnas Semánticas:
* encapsulan intención y contexto
* facilitan interpretación precisa
* son agnósticas del lenguaje
* soportan interacciones complejas
* permiten mejor codificación de memoria

### 3️⃣ **Límites del Sistema**

#### Lo que el motor *nunca* puede hacer
* Ejecutar efectos secundarios directamente
* Modificar su propio código o Seed
* Acceder a internet sin capacidad explícita
* Compartir memoria entre instancias
* Omitir verificaciones de permiso

#### Lo que debe *siempre* requerir confirmación
* Acceder a datos sensibles del usuario
* Ejecutar capacidades de alto riesgo (ej: transacciones financieras, acciones físicas)
* Manejar comandos destructivos (ej: eliminar datos, apagar sistemas)
* Reemplazar datos
* Acciones irreversibles no de solo lectura

#### Lo que se delega a los módulos

* Todas las operaciones de I/O externo
* Llamadas a API
* Interacciones con hardware
* Recepción de comandos e información del usuario
* Ejecución de comandos de capacidad

---

## ✅ Criterios de Salida (NO Avanzar Sin Estos)

**Puedes explicar ANIMA en 2 minutos sin mencionar personalidad**

ANIMA es un motor de IA privado diseñado para alojar identidades de IA duraderas y en evolución de manera segura.

En su núcleo, ANIMA separa pensamiento, identidad y acción.

El núcleo es la única parte que razona. Toma entrada estructurada, consulta memoria, aplica restricciones de identidad de un archivo Seed, verifica permisos y produce intención—nunca acciones directas.

Una Seed es una definición de identidad estática: parámetros de personalidad, límites de comportamiento, tolerancia al riesgo y qué capacidades están permitidas. No contiene memorias o código. Cada instancia ANIMA crece independientemente después de la inicialización.

La memoria es local a la instancia y falible. Almacena interacciones pasadas, estados de tareas y observaciones, e informa decisiones sin anular la política.

El núcleo solo puede actuar a través de capacidades, que son contratos declarativos que describen qué puede hacer, no cómo. Las capacidades están controladas tanto por la Seed como por las reglas de seguridad.

Cuando el núcleo produce intención, los adaptadores traducen esa intención en comandos concretos. Los adaptadores son puros y determinísticos—no hacen I/O externo ni toman decisiones.

La interacción real con el mundo ocurre solo en los módulos. Los módulos hablan con APIs, hardware, plataformas o streams, y ejecutan comandos sin razonar.

La seguridad envuelve el sistema de extremo a extremo: autenticación antes del razonamiento y aplicación de política antes de la ejecución.

Este diseño permite que ANIMA soporte asistentes privados, personas de stream, robots y herramientas—todos usando el mismo motor—mientras mantiene la identidad aislada, el comportamiento auditable y las acciones seguras.

---

## ⚠️ Trampas de la Fase 0

* Escribir código sin separación clara de responsabilidades
* Dejar que los módulos decidan el comportamiento
* Dejar que la memoria anule la política
* Confundir Seed con Memoria
* Tratar los adaptadores como opcionales


