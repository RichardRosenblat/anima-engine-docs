# LICENSING_MODEL.md

## Propósito

Este documento explica **cómo se licencia ANIMA**, qué se intercambia y qué explícitamente *no* se reclama.

El modelo de licenciamiento existe para:

* permitir desarrollo sostenible
* hacer cumplir límites de capacidad
* proteger la propiedad del usuario sobre identidad y memoria

No está diseñado para controlar el comportamiento, extraer datos o bloquear a los usuarios.

---

## Lo Que Se Vende

Cuando adquieres una licencia ANIMA, **no estás comprando una personalidad de IA**.

Recibes:

### 1. Derechos de Ejecución

El derecho a ejecutar el motor ANIMA bajo condiciones definidas.

Esto incluye:

* Acceso a actualizaciones del motor
* Correcciones de errores y mejoras de seguridad
* Compatibilidad con modelos soportados

Los derechos de ejecución son **limitados por tiempo o nivel**, no propiedad perpetua de binarios.

---

### 2. Acceso a Capacidades

Las licencias pueden habilitar:

* Módulos de capacidad específicos
* Rutas de ejecución de mayor riesgo
* Aumento de concurrencia de tareas
* Soporte expandido de adaptadores

Las capacidades son:

* Explícitas
* Descubribles
* Aplicadas en tiempo de ejecución

Una licencia **nunca otorga poder ilimitado** — solo levanta restricciones predefinidas.

---

### 3. Seeds (Semillas)

Algunas licencias incluyen:

* Seeds de identidad oficiales
* Perfiles iniciales curados
* Actualizaciones y revisiones de seeds

Las Seeds son:

* Datos, no código
* Versionadas
* Firmadas criptográficamente

Una seed otorga una **configuración inicial**, no una personalidad permanente.

---

## Lo Que No Se Vende

### 1. Memoria del Usuario

ANIMA **no** reclama propiedad sobre:

* Historial de conversaciones
* Memoria local de la instancia
* Desarrollo narrativo
* Preferencias o relaciones

La memoria pertenece al **propietario de la instancia**.

El licenciamiento no otorga:

* Acceso a la memoria
* Reutilización de memoria
* Agregación de memoria
* Derechos de entrenamiento

---

### 2. Propiedad de Identidad

ANIMA no afirma propiedad sobre:

* Personalidad emergente
* Evolución de identidad a largo plazo
* Seeds creadas por el usuario o derivadas

Lo que crece dentro de una instancia:

> pertenece al propietario de esa instancia.

---

### 3. Datos del Usuario

ANIMA no requiere:

* Almacenamiento de memoria centralizado
* Operación solo en la nube
* Envío de datos para validación de licenciamiento

---

## Comportamiento Offline

ANIMA está diseñada para funcionar **offline**.

Cuando offline:

* Las memorias existentes permanecen accesibles
* Las capacidades previamente desbloqueadas permanecen disponibles (dentro de la política)
* No ocurre ninguna nueva verificación de licencia

Si una licencia expira:

* ANIMA no borra la memoria
* ANIMA no bloquea al usuario
* ANIMA se degrada graciosamente (ej: reducción de capacidad)

No hay "interruptor de apagado".

---

## Por Qué Esto No Es DRM

DRM tradicional:

* Restringe acceso a contenido poseído
* Asume usuarios hostiles
* Castiga el uso legítimo
* Oculta la aplicación

El modelo de licenciamiento de ANIMA:

* Controla **rutas de ejecución**, no contenido
* Trata a los usuarios como operadores, no adversarios
* Es transparente y auditable
* Nunca mantiene la memoria como rehén

El licenciamiento controla **lo que el motor puede hacer**, no lo que el usuario posee.

---

## Control de Capacidad vs Propiedad

Una distinción útil:

* **Propiedad** → memoria, identidad, evolución de la instancia
* **Licenciamiento** → derechos de ejecución, exposición de capacidad

Estas preocupaciones están deliberadamente separadas.

ANIMA aplica esta separación a nivel arquitectónico.

---

## Modos de Falla

Si los sistemas de licenciamiento fallan:

* La memoria permanece intacta
* Las instancias permanecen legibles
* Los valores predeterminados de seguridad se activan
* No se elimina ningún dato

La pérdida de licencia nunca resulta en pérdida de identidad.

---

## Resumen

El modelo de licenciamiento de ANIMA existe para:

* Sostener el desarrollo
* Aplicar límites de seguridad
* Permitir acceso modular a capacidades

**No** existe para:

* Extraer datos del usuario
* Controlar la evolución de identidad
* Imponer escasez artificial
* Reemplazar propiedad por permiso

No alquilas las memorias de tu ANIMA.

Licencias **cómo puede actuar el motor**.
