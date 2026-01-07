# Divisão Adaptador–Atuador (De Propriedade do Módulo, Fora do Processo)

Este documento especifica como os Módulos são estruturados para proteger o Núcleo de código externo enquanto preserva contratos limpos.

## Estrutura

Exemplo de pacote de módulo:
- adapter/ (mapeamento puro e validação de schema)
- actuator/ (execução com efeitos; APIs, hardware)
- capability_contract/ (JSON Schemas + manifesto; versionado)
- transport/ (servidor gRPC; mTLS; aplicação de lease)
- proto/ (apenas IDL; codegen usado dentro do processo do módulo)

## Entrada (Entradas → Núcleo)

- Runtime do módulo captura sinais (texto, áudio, eventos de plataforma).
- Adaptador mapeia sinais para eventos de entrada com envelope ADR-004 e publica via gRPC.
- Núcleo ingere eventos (nunca IO bruto).

## Saída (Intenção do Núcleo → Comando do Módulo)

- Núcleo produz uma Intenção com URN de capacidade + payload.
- Núcleo chama o gateway de capacidade no Adaptador do Módulo.
- Adaptador valida payload contra JSON Schema de capacidade, mapeia para comando do Atuador, aplica escopo de lease e invoca Atuador.
- Atuador executa e emite eventos.

## Descoberta & Confiança

- Módulos apresentam um manifesto com:
  - URN do módulo
  - URNs de capacidade e versões
  - hashes de JSON Schema
  - Tipo de Módulo declarado (ADR-003: I/II/III)
- Núcleo valida manifesto sobre mTLS no handshake; nenhum código carregado do Módulo.
- Todas as solicitações carregam LeaseID/Epoch; epochs obsoletos são rejeitados.

## Observabilidade

- Tanto Adaptador quanto Atuador emitem eventos ADR-004:
  - capability.invoked
  - capability.completed
  - capability.interrupted
  - validation.failed
- Todos os eventos incluem execution_id, trace_id, span_id e source=module:<name>.

## Por Que Isso É Seguro

- Núcleo processa apenas eventos, nunca código de terceiros.
- Tradução e execução estão fora do processo.
- Schemas declarativos previnem payloads ad-hoc.
- Leases e epochs previnem replay/escalação.
