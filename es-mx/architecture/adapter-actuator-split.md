# División Adaptador–Actuador (De Propiedad del Módulo, Fuera del Proceso)

Este documento especifica cómo se estructuran los Módulos para proteger el Núcleo de código externo mientras se preservan contratos limpios.

## Estructura

Ejemplo de paquete de módulo:
- adapter/ (mapeo puro y validación de schema)
- actuator/ (ejecución con efectos; APIs, hardware)
- capability_contract/ (JSON Schemas + manifiesto; versionado)
- transport/ (servidor gRPC; mTLS; aplicación de lease)
- proto/ (solo IDL; codegen usado dentro del proceso del módulo)

## Entrada (Entradas → Núcleo)

- Runtime del módulo captura señales (texto, audio, eventos de plataforma).
- Adaptador mapea señales a eventos de entrada con sobre ADR-004 y publica vía gRPC.
- Núcleo ingiere eventos (nunca IO crudo).

## Salida (Intención del Núcleo → Comando del Módulo)

- Núcleo produce una Intención con URN de capacidad + payload.
- Núcleo llama al gateway de capacidad en el Adaptador del Módulo.
- Adaptador valida payload contra JSON Schema de capacidad, mapea a comando del Actuador, aplica alcance de lease e invoca Actuador.
- Actuador ejecuta y emite eventos.

## Descubrimiento & Confianza

- Los módulos presentan un manifiesto con:
  - URN del módulo
  - URNs de capacidad y versiones
  - hashes de JSON Schema
  - Tipo de Módulo declarado (ADR-003: I/II/III)
- Núcleo valida manifiesto sobre mTLS en el handshake; ningún código cargado del Módulo.
- Todas las solicitudes llevan LeaseID/Epoch; epochs obsoletos son rechazados.

## Observabilidad

- Tanto Adaptador como Actuador emiten eventos ADR-004:
  - capability.invoked
  - capability.completed
  - capability.interrupted
  - validation.failed
- Todos los eventos incluyen execution_id, trace_id, span_id y source=module:<name>.

## Por Qué Esto Es Seguro

- Núcleo procesa solo eventos, nunca código de terceros.
- Traducción y ejecución están fuera del proceso.
- Schemas declarativos previenen payloads ad-hoc.
- Leases y epochs previenen replay/escalación.
