# Divisão Adapter–Actuator (Propriedade do Módulo, Fora de Processo)

**ADRs Relacionados:**
* [ADR-003: Protocolo de Comunicação Core ↔ Módulo, Tipos de Módulos e Ciclo de Vida de Lease](../adr/ADR-003.md)
* [ADR-004: Observabilidade, Registro de Eventos e Rastreabilidade de Execução](../adr/ADR-004.md)
* [ADR-005: Modelo de Interrupção & Preempção](../adr/ADR-005.md)
* [ADR-010: Divisão Adapter–Actuator com Fronteira de Processo Rigorosa](../adr/ADR-010.md)
* [ADR-011: Arquitetura de Entrada Baseada em Eventos](../adr/ADR-011.md)

---

## Visão Geral

Este documento especifica como os Módulos são estruturados para proteger o Core de código externo enquanto preserva contratos limpos.

Cada Módulo é dividido em duas partes, ambas executando **fora de processo** do Core:

* **Adapter** — Tradução pura e validação de schema
* **Actuator** — Execução com efeitos que toca APIs/hardware

O Core **nunca carrega ou executa código de propriedade do módulo**. Toda interação Core ↔ Módulo ocorre sobre **gRPC com mTLS** e **leases emitidos pelo Core**.

---

## Princípio de Segurança Central

> **O Core executa apenas código confiável, de propriedade do motor.**

* Adapters e Actuators executam em processos de propriedade do módulo
* Toda comunicação usa gRPC sobre mTLS com identidades atestadas (Core URN, Module URN) e leases emitidos pelo Core
* Sem importações dinâmicas, plugins ou execução Python de módulos dentro do Core

---

## Estrutura de Pacote de Módulo

Exemplo de pacote de módulo:

```
module-package/
├── adapter/
│   ├── input/  (sinais → eventos input.*; mapeamento puro)
│   └── output/ (intenção → comandos; mapeamento puro + validação de schema)
├── actuator/ (execução com efeitos: APIs/hardware)
├── capability_contract/ (JSON Schemas + manifesto)
├── transport/ (servidor/cliente gRPC; mTLS; imposição de lease)
└── proto/ (IDL para serviços CapabilityGateway e Adapter)
```

---

## Adapter (Lado do Módulo, Tradução Pura)

### Responsabilidades

**Caminho de entrada:**
* Mapeia sinais capturados → eventos de entrada do Core

**Caminho de saída:**
* Mapeia Intenção do Core (URN de capacidade + payload) → schema de comando do módulo

### Deveres Principais

* Tradução determinística
* Validação de JSON Schema contra o contrato de capacidade
* Imposição de escopo de lease em chamadas de saída

### Proibições

* Sem planejamento, sem política
* Sem I/O externo além de gRPC para Core ou invocação de actuator local
* Sem acesso a memória ou internals do Core

---

## Actuator (Lado do Módulo, Execução com Efeitos)

### Responsabilidades

* Executa efeitos colaterais do mundo real (APIs, hardware, plataformas)
* Impõe escopo de lease e políticas de interrupção (ADR-005)
* Emite eventos de observabilidade com envelope ADR-004
* Contém zero lógica ou raciocínio do Core

---

## Fluxo de Entrada (Inputs → Core)

1. Runtime do módulo captura sinais (texto, áudio, eventos de plataforma)
2. Adapter mapeia sinais para eventos de entrada com envelope ADR-004
3. Adapter publica eventos via gRPC
4. Core ingere eventos (nunca IO bruto)

Tipos de eventos (veja [Arquitetura de Eventos](event-architecture.md)):
* `input.nl` — linguagem natural (pré-semântica)
* `input.system` — fatos não-linguísticos, mecânicos/de sistema
* `input.semantic` — significado afirmado pós-interpretação

---

## Fluxo de Saída (Intenção do Core → Comando do Módulo)

1. Core produz uma Intenção com URN de capacidade + payload
2. Core chama o gateway de capacidade no Adapter do Módulo
3. Adapter valida payload contra JSON Schema de capacidade
4. Adapter mapeia para comando do Actuator
5. Adapter impõe escopo de lease
6. Adapter invoca Actuator
7. Actuator executa e emite eventos

---

## Processamento de Linguagem Natural em Módulos

Módulos que lidam com entrada e saída de linguagem natural PODEM enviar uma implementação NLP ou delegar responsabilidades NLP para o Arcuate do Core (veja [Topologia de Modelos de IA](ai-model-topology.md)).

### Entrada de Linguagem Natural

**Opção A — NLP de propriedade do módulo:**
* O Adapter do módulo realiza NLP local para transformar eventos `input.nl` → `input.semantic`
* Restrições:
  * Apenas tradução; sem planejamento ou política
  * Incluir confiança e proveniência em payloads semânticos
  * Sem acesso a memória do Core; proveniência reflete "módulo"

**Opção B — Delegar ao Arcuate:**
* O Adapter do módulo publica eventos `input.nl` para o Core
* O Arcuate do Core consome `input.nl` e produz `input.semantic`
* Core raciocina sobre eventos semânticos
* Restrições:
  * Adapter permanece puro; sem NLP embutido
  * Fronteira de processo preservada; sem código de plugin no Core

### Saída de Linguagem Natural (e.g., TTS, entrega de chat)

**Opção A — NLG de propriedade do módulo:**
* O Adapter do módulo mapeia intenções do Core para schemas de comando de saída e realiza NLG local (realização de superfície) antes da execução do Actuator
* Restrições:
  * NLG é tradução; sem planejamento, sem acesso a memória
  * Eventos de observabilidade DEVEM capturar intenção, metadata de realização e resultado de execução

**Opção B — Delegar ao Arcuate:**
* O Core produz a superfície linguística via Arcuate do conteúdo semântico de uma intenção
* O Adapter do módulo traduz o texto realizado para o comando do módulo
* O Actuator executa (e.g., falar/enviar)
* Restrições:
  * Adapter permanece puro; Actuator realiza efeitos colaterais
  * Uso de Arcuate segue regras de isolamento ADR-009

Em todos os casos:
* Adapters permanecem mapeamento puro + validação
* Actuators realizam IO e efeitos sob leases válidos
* Eventos semânticos DEVEM incluir campos de confiança e proveniência
* Linguagem natural NÃO DEVE acionar execução direta; o pipeline permanece Entrada → Interpretação → Intenção → Validação/Confirmação → Execução

---

## Descoberta & Confiança

Módulos apresentam um manifesto com:
* URN do Módulo
* URNs de Capacidade e versões
* Hashes de JSON Schema
* Tipo de Módulo Declarado (veja [Tipos de Módulos e Leases](module-types-and-leases.md): Tipo I/II/III)

Core valida manifesto sobre mTLS no handshake; **nenhum código carregado do Módulo**.

Todas as requisições carregam LeaseID/Epoch; epochs obsoletos são rejeitados.

---

## Protocolo & Contratos

### Entrada (Módulo → Core)
* Adapter publica eventos de entrada e tipos de eventos

### Saída (Core → Módulo)
* Core chama Adapter via um Capability Gateway genérico:
  * `capability_urn` + `schema_version` + `payload` (validado por adapter)
  * `LeaseMeta` (lease_id, epoch)
  * `ExecMeta` (execution/trace/span)

### Contrato de Capacidade

JSON Schemas declarativos para payloads de comando contendo:
* Classe de efeito colateral do método (reversível/irreversível)
* Política de interrupção (soft-stop/hard-stop/checkpoint/non-interruptible)
* Duração máxima de lease

---

## Observabilidade

Tanto Adapter quanto Actuator emitem eventos estruturados conforme ADR-004:

Tipos de eventos:
* `capability.invoked`
* `capability.completed`
* `capability.interrupted`
* `validation.failed`

Todos os eventos incluem:
* `execution_id`, `trace_id`, `span_id`, `thread_id`, `timestamp`
* `source=module:<name>`
* `type=<event.type.identifier>`
* `payload=<structured fields only>`

Sem logging de texto livre como autoridade; eventos são a fonte da verdade.

---

## Invariantes de Segurança

* Sem código de plugin no Core — jamais
* Toda execução de módulo requer um lease válido com epoch atual
* Promoção/demissão de escopo ocorre apenas via atualizações de lease emitidas pelo Core
* Zero-Lease State: módulos são inertes (Tipo II em standby ou Tipo I termina)
* Adapters não podem ampliar escopo ou desviar do controle do Core

---

## Por Que Isto é Seguro

* Core processa apenas eventos, nunca código de terceiros
* Tradução e execução são fora de processo
* Schemas declarativos previnem payloads ad-hoc
* Leases e epochs previnem replay/escalação
* Elimina risco de injeção de código no Core
* Preserva limites hexagonais e testabilidade
* Divisão clara de responsabilidade (traduzir vs executar)
* Forte auditabilidade via eventos estruturados

---

## Resumo

> **Core raciocina e supervisiona. Módulos adaptam e agem.**
> **Nenhum código de terceiros executa dentro do Core.**
