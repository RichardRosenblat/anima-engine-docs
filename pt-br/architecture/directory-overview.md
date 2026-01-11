# Documentação de Arquitetura ANIMA

Esta pasta contém documentação arquitetural detalhada para o Motor ANIMA, derivada dos Architecture Decision Records (ADRs) e princípios de design.

---

## Documentos Principais de Arquitetura

### [Visão Geral da Arquitetura ANIMA](anima-architecture.md)
Visão geral abrangente de toda a arquitetura ANIMA, incluindo todas as camadas e conceitos-chave.

**Tópicos cobertos:**
* Separação de Motor vs. Identidade
* Implementação de Arquitetura Hexagonal
* Observabilidade e entrada baseadas em eventos
* Modelo de Kernel Cognitivo
* Fluxo completo do sistema

---

## Documentação Detalhada de Tópicos

### [Sistema de Seeds](seed-system.md)
**Relacionado:** [ADR-001](../adr/ADR-001.md)

Explica como ANIMA separa o motor da identidade através de Seeds declarativos.

**Tópicos cobertos:**
* O que é um Seed
* Motor vs. Identidade
* Invariantes arquiteturais
* Casos de uso (Instâncias privadas, ANIMA Prime)

---

### [Tipos de Módulo e Leases](module-types-and-leases.md)
**Relacionado:** [ADR-003](../adr/ADR-003.md)

Detalha o modelo de autorização baseado em lease e três tipos de módulos.

**Tópicos cobertos:**
* Comunicação gRPC sobre mTLS
* Autorização baseada em lease
* Épocas de lease e anti-dessincronização
* Módulos Tipo I, II e III
* Estado Zero-Lease
* Regras de persistência de estado

---

### [Arquitetura de Eventos](event-architecture.md)
**Relacionado:** [ADR-004](../adr/ADR-004.md), [ADR-011](../adr/ADR-011.md)

Descreve a arquitetura unificada baseada em eventos para observabilidade e processamento de entrada.

**Tópicos cobertos:**
* Modelo de observabilidade baseado em eventos
* Particionamento de execução
* Estrutura e campos de eventos
* Categorias canônicas de eventos (input.nl, input.system, input.semantic)
* Concorrência e multithreading
* Participação de módulos

---

### [Kernel Cognitivo](cognitive-kernel.md)
**Relacionado:** [ADR-005](../adr/ADR-005.md), [ADR-008](../adr/ADR-008.md)

Explica como o ANIMA Core opera como um kernel cognitivo multitarefa.

**Tópicos cobertos:**
* Tarefas como unidades de execução gerenciadas
* Modelo de interrupção cooperativa
* Interrupções como eventos de primeira classe
* Períodos de execução e foco
* Políticas de interruptibilidade
* Kernel vs. espaço de Usuário
* Modelo de multitarefa

---

### [Domínio e Infraestrutura](domain-and-infrastructure.md)
**Relacionado:** [ADR-002](../adr/ADR-002.md), [ADR-006](../adr/ADR-006.md), [ADR-007](../adr/ADR-007.md)

Formaliza a implementação da Arquitetura Hexagonal em ANIMA.

**Tópicos cobertos:**
* Princípio central (domínios nunca falam com o mundo externo)
* Regras de direção de dependência
* Formas de dependência entre domínios aprovadas
* Tradução de erros entre domínios
* Esquemas auto-validáveis
* Camada de infraestrutura e adaptadores
* Composição em tempo de execução

---

### [Topologia do Modelo de IA](ai-model-topology.md)
**Relacionado:** [ADR-009](../adr/ADR-009.md)

Detalha a topologia de modelo de IA de dois papéis: Córtex e Arcuate.

**Tópicos cobertos:**
* Córtex (modelo cognitivo obrigatório)
* Arcuate (modelo de PLN opcional)
* PLN como uma responsabilidade configurável
* Configurações suportadas (modelo único, modelo duplo, somente Córtex)
* Arquitetura baseada em interface
* Restrições e invariantes

---

### [Divisão Adaptador-Atuador](adapter-actuator-split.md)
**Relacionado:** [ADR-003](../adr/ADR-003.md), [ADR-004](../adr/ADR-004.md), [ADR-005](../adr/ADR-005.md), [ADR-010](../adr/ADR-010.md), [ADR-011](../adr/ADR-011.md)

Especifica como os Módulos são estruturados com Adaptadores e Atuadores executando fora de processo.

**Tópicos cobertos:**
* Princípio de segurança central (nenhum código de terceiros no Core)
* Estrutura de pacote de módulo
* Responsabilidades do adaptador (tradução pura)
* Responsabilidades do atuador (execução com efeitos)
* Fluxos de entrada e saída
* Processamento de linguagem natural em módulos
* Descoberta e confiança
* Observabilidade
* Invariantes de segurança

---

## Princípios Arquiteturais Chave

1. **Motor ≠ Identidade** — A personalidade vem de Seeds, não do motor
2. **Nenhum código de terceiros no Core** — Todos os módulos executam fora de processo
3. **Autorização baseada em lease** — Toda execução requer leases válidos
4. **Domínios nunca importam infraestrutura** — Fronteiras hexagonais limpas
5. **Tudo baseado em eventos** — Não há logs tradicionais, apenas eventos estruturados
6. **Memória com escopo de instância** — Nenhum compartilhamento entre instâncias
7. **Córtex obrigatório, Arcuate opcional** — Separação de cognição vs. linguagem
8. **Core supervisiona, não executa** — Modelo de kernel cognitivo
9. **Interrupção cooperativa** — Todas as ações são interrompíveis por design
10. **Eventos são a fonte da verdade** — Fatos imutáveis e auditáveis

---

## Architecture Decision Records (ADRs)

Para os registros de decisão formais que definem esta arquitetura, veja a [pasta ADR](../adr/):

* [ADR-001](../adr/ADR-001.md) — Separação do Motor ANIMA e Identidade via Sistema de Seeds
* [ADR-002](../adr/ADR-002.md) — Esquemas Auto-Validáveis com Restrições Somente Internas
* [ADR-003](../adr/ADR-003.md) — Protocolo de Comunicação Core ↔ Módulo, Tipos de Módulo e Ciclo de Vida de Lease
* [ADR-004](../adr/ADR-004.md) — Observabilidade, Registro de Eventos e Rastreabilidade de Execução
* [ADR-005](../adr/ADR-005.md) — Modelo de Interrupção e Preempção
* [ADR-006](../adr/ADR-006.md) — Regras de Dependência e Compartilhamento de Domínio
* [ADR-007](../adr/ADR-007.md) — Camada de Infraestrutura e Regras de Adaptador
* [ADR-008](../adr/ADR-008.md) — ANIMA Core se Comporta como um Kernel Cognitivo
* [ADR-009](../adr/ADR-009.md) — Topologia de Modelo de IA Configurável com Córtex Obrigatório e Arcuate Opcional
* [ADR-010](../adr/ADR-010.md) — Divisão Adaptador–Atuador com Fronteira de Processo Estrita
* [ADR-011](../adr/ADR-011.md) — Arquitetura de Entrada Baseada em Eventos

---

## Referência Rápida

### Fluxo do Sistema

```
Usuário/Ambiente
  ↓ (captura)
Módulo
  ↓ (traduzir para eventos)
Adaptador
  ↓ (input.nl / input.system / input.semantic)
Segurança
  ↓
Kernel Cognitivo (Córtex + Arcuate opcional)
  ↔ Memória / Seed
  ↓ (intenção + URN de capacidade)
Segurança (validar)
  ↓
Adaptador (validar esquema)
  ↓
Atuador (executar)
  ↓
Efeito + Eventos
```

### Tipos de Módulo

* **Tipo I** — Privado Efêmero (vinculado a lease, termina em Zero-Lease)
* **Tipo II** — Privado Residente (standby em Zero-Lease, Core único)
* **Tipo III** — Compartilhado Residente (multi-tenant, governado por infraestrutura)

### Tipos de Evento

* **input.nl** — Linguagem natural (pré-semântico)
* **input.system** — Fatos do sistema/mecânicos
* **input.semantic** — Significado afirmado pós-interpretação
* **capability.invoked** — Execução de capacidade iniciada
* **capability.completed** — Execução de capacidade concluída
* **capability.interrupted** — Execução de capacidade interrompida

---

## Navegação

* [README Principal](../README.md)
* [Índice de ADR](../adr/)
* [Visão](../vision/vision.md)
* [Fundamentos](../foundations/canonical-glossary.md)
* [Integridade da Memória](../safety/memory-integrity.md)
* [Modelo de Segurança](../safety/safety-model.md)
* [Modelo de Ameaças](../safety/threat-model.md)
