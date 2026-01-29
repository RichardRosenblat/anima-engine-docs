# Guía de Mantenimiento de Documentación

**Propósito:** Esta guía explica cómo se mantiene, revisa y actualiza la documentación de ANIMA.

---

## Principios Rectores

La documentación de ANIMA sigue estos principios centrales:

1. **Claridad Primero** - Precisión y comprensibilidad sobre brevedad
2. **Terminología Canónica** - Las definiciones en el [Canonical Glossary](foundations/canonical-glossary.md) nunca deben desviarse
3. **Sin Duplicación de Autoridad** - Cada concepto tiene una fuente autoritativa
4. **Honestidad Arquitectónica** - Documentar las decisiones como son, no como desearíamos que fueran
5. **Referencias Cruzadas Deliberadas** - Enlazar a fuentes canónicas en lugar de repetir contenido

---

## Cadencia de Revisión de Documentación

### Calendario Recomendado

* **Revisiones Trimestrales** - Verificar:
  * Referencias cruzadas rotas
  * Descripciones arquitectónicas desactualizadas
  * Desviación de terminología
  * Inconsistencias con la implementación (una vez que el motor sea público)

* **Antes de Lanzamientos Mayores** - Verificar:
  * Todos los ADRs están actualizados
  * La documentación de arquitectura coincide con la implementación
  * El modelo de seguridad refleja las restricciones reales
  * Los ejemplos y especificaciones son precisos

* **Después de Cambios Arquitectónicos Significativos** - Actualizar:
  * ADRs afectados (o crear nuevos)
  * Documentación de arquitectura
  * Canonical Glossary (si cambia la terminología)
  * Referencias cruzadas en documentos relacionados

### Lista de Verificación de Revisión

Al revisar la documentación:

- [ ] ¿Funcionan todas las referencias cruzadas?
- [ ] ¿La terminología es consistente con el Canonical Glossary?
- [ ] ¿Los ADRs siguen siendo precisos o necesitan actualizaciones de estado?
- [ ] ¿Los ejemplos reflejan la arquitectura actual?
- [ ] ¿Los non-goals siguen siendo precisos?
- [ ] ¿El tono es consistente (preciso, calmado, profesional, con opinión cuando es apropiado)?
- [ ] ¿Hay explicaciones duplicadas que deberían consolidarse?
- [ ] ¿Los diagramas (basados en texto o visuales) siguen siendo precisos?

---

## Cuándo Crear un Nuevo ADR vs. Actualizar Uno Existente

### Crear un Nuevo ADR Cuando:

* **Se toma una nueva decisión arquitectónica** que afecta la estructura, comportamiento o límites del sistema
* **Se agrega o elimina una restricción significativa** (ej., nuevo límite de seguridad, cambio en modelo de capacidades)
* **Se introduce un componente mayor** (ej., nueva capa de memoria, nuevo tipo de módulo)
* **Un ADR existente es reemplazado** (marca el antiguo como "Reemplazado por ADR-XXX")

### Actualizar un ADR Existente Cuando:

* **Clarificando decisiones existentes** sin cambiar la decisión misma
* **Agregando contexto o razón fundamental** que estaba implícita pero no escrita
* **Corrigiendo errores o erratas** en la descripción
* **Agregando referencias cruzadas** a documentos relacionados

### NO Actualizar ADRs Para:

* Detalles de implementación (estos van en comentarios de código o documentos de implementación)
* Elecciones tácticas que no afectan la arquitectura
* Soluciones temporales
* Configuraciones específicas de plataforma

---

## Ciclo de Vida de ADR y Valores de Estado

Los ADRs usan estos valores de estado:

* **Proposed** - En discusión, aún no adoptado
* **Accepted** - Adoptado y guiando la implementación
* **Deprecated** - Ya no recomendado, pero aún no reemplazado
* **Superseded** - Reemplazado por un ADR más nuevo (incluir referencia: "Reemplazado por ADR-XXX")
* **Rejected** - Considerado pero explícitamente rechazado (mantener para contexto histórico)

**Las actualizaciones de estado** son apropiadas cuando:
* Una decisión arquitectónica ya no es válida
* Un enfoque mejor reemplaza uno anterior
* La implementación revela que la decisión era incorrecta

**Incluir una nota** explicando por qué cambió el estado y enlazando al ADR que lo reemplaza si aplica.

---

## Cómo Proponer Cambios en la Documentación

### Para Cambios Menores (Erratas, Formato, Enlaces Rotos)

1. **Identificar el problema** (enlace roto, errata, inconsistencia de formato)
2. **Verificar que no sea intencional** (revisar documentos relacionados para contexto)
3. **Enviar un pull request** con:
   * Descripción clara de qué estaba mal
   * Qué se cambió
   * Por qué el cambio es correcto

### Para Clarificaciones y Adiciones

1. **Verificar duplicación de autoridad** - ¿Este contenido ya está cubierto en otro lugar?
2. **Identificar la fuente canónica** - ¿Esto debería agregarse a un documento existente o ser nuevo?
3. **Seguir el tono existente** - Preciso, calmado, profesional, con opinión cuando es apropiado
4. **Usar terminología canónica** - Referir el [Canonical Glossary](foundations/canonical-glossary.md)
5. **Agregar referencias cruzadas** - Enlazar a fuentes autoritativas relacionadas
6. **Enviar un pull request** explicando:
   * Qué vacío llena el cambio
   * Por qué este es el lugar correcto para el contenido
   * Cómo mantiene consistencia con documentos existentes

### Para Cambios Arquitectónicos o Conceptuales

1. **Discutir primero** - Los cambios arquitectónicos requieren consenso
2. **Determinar si se necesita un nuevo ADR** (ver "Cuándo Crear un Nuevo ADR" arriba)
3. **Redactar el cambio** manteniendo:
   * Consistencia con terminología canónica
   * Alineación con decisiones arquitectónicas existentes
   * Claridad y precisión
4. **Actualizar referencias cruzadas** - Encontrar todos los documentos que referencian el concepto cambiado
5. **Enviar un pull request** con:
   * Razón fundamental para el cambio
   * Impacto en la documentación existente
   * Lista de referencias cruzadas afectadas

---

## Actualizaciones del Canonical Glossary

El [Canonical Glossary](foundations/canonical-glossary.md) es **autoritativo**. Los cambios requieren cuidado especial.

### Cuándo Actualizar el Glosario

* Se introduce un nuevo componente arquitectónico
* Una definición existente es ambigua o incompleta
* Se detecta desviación de terminología
* Un ADR introduce nuevos términos canónicos

### Cómo Actualizar el Glosario

1. **Verificar que el término es arquitectónico** - Los detalles tácticos o de implementación no pertenecen aquí
2. **Verificar conflictos** - ¿Esto entra en conflicto con definiciones existentes?
3. **Escribir la definición con precisión** - Ser completo pero conciso
4. **Agregar referencias cruzadas** - Enlazar a ADRs y documentos de arquitectura relacionados
5. **Actualizar todo uso** - Buscar el término en la documentación y asegurar uso consistente
6. **Obtener revisión** - Los cambios en el glosario afectan todo el conjunto de documentación

**Nunca** cambiar una definición canónica existente sin:
* Documentar la razón del cambio
* Actualizar todas las referencias a ese término
* Considerar si se necesita una actualización de estado de ADR

---

## Estructura y Organización de la Documentación

### Estructura de Directorios Actual

```
/
├── README.md                  # Punto de entrada y vista general de alto nivel
├── GETTING_STARTED.md         # Guía de navegación para nuevos lectores
├── FAQ.md                     # Preguntas comunes con respuestas concisas
├── DOCUMENTATION_MAINTENANCE.md # Este documento
├── adr/                       # Registros de Decisiones de Arquitectura (numerados)
├── architecture/              # Documentación detallada de arquitectura
├── foundations/               # Principios canónicos y glosario
├── vision/                    # Visión, objetivos y non-goals
├── governance/                # Licencias y viabilidad del proyecto
├── safety/                    # Modelo de seguridad, modelo de amenazas, integridad de memoria
├── specs/                     # Especificaciones técnicas (URN, JSRS)
├── roadmaps/                  # Hoja de ruta de desarrollo
└── announcements/             # Anuncios del proyecto
```

### Agregar Nuevos Documentos

**Antes de agregar un nuevo documento:**

1. **Verificar si el contenido pertenece a un documento existente** - Evitar fragmentación
2. **Determinar el directorio apropiado** - Seguir la estructura existente
3. **Asegurar que tenga un propósito claro y único** - Sin duplicación de autoridad
4. **Usar un nombre de archivo descriptivo** - Minúsculas, separado por guiones (ej., `nuevo-concepto.md`)

**Al agregar un nuevo documento:**

1. Incluir una **declaración de propósito** al inicio
2. Agregar **referencias cruzadas** a documentos relacionados
3. Actualizar documentos de navegación (README, GETTING_STARTED.md, READMEs de directorios relevantes)
4. Usar terminología canónica
5. Seguir convenciones de formato existentes

### Eliminar o Renombrar Documentos

**Evitar renombrar o eliminar documentos.** Esto rompe enlaces externos e interrumpe la navegación.

**Si un documento debe renombrarse:**
* Crear una redirección o stub en la ubicación antigua apuntando a la nueva
* Actualizar todas las referencias cruzadas
* Documentar el cambio en anuncios

**Si un documento debe eliminarse:**
* Asegurar que el contenido se preserva en otro lugar (si sigue siendo válido)
* Actualizar todas las referencias cruzadas
* Considerar dejar un stub explicando dónde se movió el contenido

---

## Mantenimiento de Referencias Cruzadas

Las referencias cruzadas son críticas para la navegación. Los enlaces rotos socavan la calidad de la documentación.

### Mejores Prácticas

* **Usar rutas relativas** - `[Canonical Glossary](foundations/canonical-glossary.md)`
* **Enlazar a fuentes autoritativas** - No repetir contenido, enlazar a él
* **Usar texto de enlace descriptivo** - No "haz clic aquí", sino "consulta [Seed System](architecture/seed-system.md)"
* **Verificar enlaces antes de hacer commit** - Especialmente después de movimientos o renombres de archivos

### Encontrar Enlaces Rotos

Verificar periódicamente enlaces rotos:

```bash
# Encontrar todos los enlaces markdown
grep -r "\[.*\](.*\.md)" *.md */*.md

# Verificar si los archivos referenciados existen
# (Verificación manual o herramienta automatizada de verificación de enlaces)
```

---

## Directrices de Tono y Estilo

La documentación de ANIMA usa un tono **preciso, calmado, profesional y con opinión**.

### Hacer:
* Ser preciso e inequívoco
* Declarar decisiones de manera clara y confiada
* Explicar razón fundamental sin ser defensivo
* Usar "ella" para ANIMA (cuando se refiere a una instancia, no al motor)
* Usar terminología técnica correctamente
* Ser honesto sobre limitaciones y restricciones

### No Hacer:
* Usar lenguaje de marketing o exageración
* Evadir innecesariamente ("tal vez", "quizás", "podría")
* Disculparse por decisiones de diseño
* Usar humor a expensas de la claridad
* Suavizar límites o restricciones de seguridad
* Introducir nueva terminología sin definiciones canónicas

### Ejemplos

**Bien:**
> "ANIMA no está diseñado para operar como un agente autónomo en internet. Este es un límite explícito elegido para proteger la seguridad, no una limitación temporal."

**Mal:**
> "Aún no estamos listos para acceso a internet, pero podríamos agregarlo más adelante si podemos hacerlo lo suficientemente seguro."

**Bien:**
> "El Cognitive Kernel supervisa la ejecución a través de módulos controlados por leases. Este invariante arquitectónico asegura que ninguna acción ocurra sin autorización criptográfica válida."

**Mal:**
> "Usamos un sistema de leases genial que más o menos asegura que las cosas estén autorizadas."

---

## Registro de Cambios de Documentación (Opcional)

Para actualizaciones mayores de documentación, considera mantener un registro de cambios ligero:

### Formato

```markdown
## [Fecha] - [Tipo de Cambio]

### Agregado
- Nuevo documento: [Título](ruta/al/documento.md)
- Nueva sección en [Documento](ruta.md): [Nombre de Sección]

### Cambiado
- Actualizado [Documento](ruta.md): [Descripción del cambio]
- Revisado estado de ADR-XXX a [Nuevo Estado]

### Corregido
- Corregidos enlaces rotos en [Documento](ruta.md)
- Corregida inconsistencia de terminología en [Documento](ruta.md)
```

### Cuándo Actualizar

* Reestructuración mayor de documentación
* Nuevos ADRs publicados
* Adiciones significativas de contenido
* Cambios de terminología en Canonical Glossary

---

## Resumen del Proceso

### Para Contribuidores de Documentación

1. **Leer primero la documentación existente** - Comprender el estado actual
2. **Seguir la guía de estilo** - Mantener consistencia
3. **Usar terminología canónica** - Verificar el Glosario
4. **Agregar referencias cruzadas** - Enlazar a fuentes autoritativas
5. **Probar todos los enlaces** - Asegurar que funcionan
6. **Mantener precisión** - Claridad sobre ingenio
7. **Obtener revisión** - Los cambios en la documentación afectan a todos los lectores

### Para Revisores de Documentación

1. **Verificar consistencia de terminología** - ¿Coincide con el Glosario?
2. **Verificar referencias cruzadas** - ¿Los enlaces son correctos y apropiados?
3. **Evaluar tono** - ¿Es preciso, calmado, profesional y con opinión?
4. **Buscar duplicación** - ¿Está dividida la autoridad entre múltiples documentos?
5. **Evaluar estructura** - ¿Es este el lugar correcto para este contenido?
6. **Considerar impacto** - ¿Cómo afecta este cambio a documentos relacionados?

---

## Contacto y Discusión

Para preguntas sobre documentación:
* Revisar issues y pull requests existentes
* Proponer cambios vía pull requests
* Incluir razón fundamental y contexto en descripciones de PR

---

**Última Actualización:** 28 de enero de 2026
