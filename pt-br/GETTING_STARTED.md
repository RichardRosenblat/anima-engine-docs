# Primeiros Passos com a Documentação ANIMA

**Objetivo:** Este guia ajuda novos leitores a navegar pela documentação ANIMA de forma eficiente.

ANIMA é um engine de IA modular e privado projetado para hospedar identidades artificiais duradouras e evolutivas sob restrições rigorosas de segurança, memória e capacidades. Esta documentação explica a arquitetura, decisões de design e fundamentos filosóficos do sistema.

---

## Para Quem É Esta Documentação?

Esta documentação atende a múltiplos públicos:

### 1. **Revisores de Arquitetura**
Você quer entender o design do sistema ANIMA, padrões arquiteturais e justificativa de design.

**Comece aqui:**
* [Vision](vision/vision.md) - Entenda o que ANIMA está tentando possibilitar
* [ANIMA Architecture](architecture/anima-architecture.md) - Visão geral abrangente do sistema
* [ADRs](adr/) - Architecture Decision Records (leia em ordem, ADR-001 até ADR-011)

### 2. **Revisores de Segurança e Governança**
Você precisa avaliar o modelo de segurança, limites de ameaças e abordagem de governança do ANIMA.

**Comece aqui:**
* [Safety Model](safety/safety-model.md) - Arquitetura de segurança e proteção
* [Threat Model](safety/threat-model.md) - Análise de ameaças e estratégias de mitigação
* [Memory Integrity](safety/memory-integrity.md) - Restrições de gerenciamento de memória
* [Non-Goals](vision/non-goals.md) - Limites explícitos e o que ANIMA se recusa a se tornar

### 3. **Futuros Implementadores**
Você pode eventualmente implementar, integrar ou estender componentes ANIMA.

**Comece aqui:**
* [Canonical Glossary](foundations/canonical-glossary.md) - Terminologia autoritativa (leia primeiro)
* [Seed System](architecture/seed-system.md) - Como identidades são inicializadas
* [Module Types and Leases](architecture/module-types-and-leases.md) - Ciclo de vida e autorização de módulos
* [Event Architecture](architecture/event-architecture.md) - Sistema de entrada e observabilidade

### 4. **Leitores Técnicos Gerais**
Você quer uma compreensão ampla do que é ANIMA e por que existe.

**Comece aqui:**
* [README](README.md) - Introdução de alto nível e FAQ
* [Vision](vision/vision.md) - Objetivos de longo prazo e fundamentos filosóficos
* [Why Not an Agent?](vision/why-not-an-agent.md) - O que torna ANIMA diferente
* [Project Charter](vision/project-charter.md) - Propósito e valores principais

---

## Ordens de Leitura Sugeridas

### Visão Rápida (15-30 minutos)
Para uma compreensão rápida dos conceitos fundamentais de ANIMA:

1. [README](README.md) - Introdução e visão geral de alto nível
2. [Vision](vision/vision.md) - Propósito e filosofia
3. [ANIMA Architecture](architecture/anima-architecture.md) - Visão geral do design do sistema
4. [Non-Goals](vision/non-goals.md) - O que ANIMA não é

### Revisão Abrangente (2-4 horas)
Para uma compreensão completa de todo o sistema:

1. **Fundamentos** (30 minutos)
   * [Vision](vision/vision.md)
   * [Project Charter](vision/project-charter.md)
   * [Canonical Glossary](foundations/canonical-glossary.md)
   * [System Boundaries](foundations/system-boundaries.md)

2. **Arquitetura** (1-2 horas)
   * [ANIMA Architecture](architecture/anima-architecture.md)
   * [Cognitive Kernel](architecture/cognitive-kernel.md)
   * [Seed System](architecture/seed-system.md)
   * [Module Types and Leases](architecture/module-types-and-leases.md)
   * [Event Architecture](architecture/event-architecture.md)

3. **Segurança e Governança** (30-45 minutos)
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)

4. **Decisões de Design** (45-60 minutos)
   * Leia ADRs em ordem: [ADR-001](adr/ADR-001.md) até [ADR-011](adr/ADR-011.md)

### Mergulho Arquitetural Profundo (4-6 horas)
Para implementadores e revisores de arquitetura que precisam de compreensão completa:

1. **Definições Canônicas Primeiro**
   * [Canonical Glossary](foundations/canonical-glossary.md) - Leia isto antes de qualquer coisa
   * [System Boundaries](foundations/system-boundaries.md)

2. **Fundamento Filosófico**
   * [Vision](vision/vision.md)
   * [Why Not an Agent?](vision/why-not-an-agent.md)
   * [Reinvention Rationale](foundations/reinvention-rationale.md)
   * [Non-Goals](vision/non-goals.md)

3. **Conjunto Completo de ADRs** (em ordem)
   * [ADR-001: Engine/Identity Split & Seed System](adr/ADR-001.md)
   * [ADR-002: Hexagonal Architecture](adr/ADR-002.md)
   * [ADR-003: Memory Layers & Integrity](adr/ADR-003.md)
   * [ADR-004: Declarative Capability System](adr/ADR-004.md)
   * [ADR-005: Interruption & Preemption Model](adr/ADR-005.md)
   * [ADR-006: Domain Dependency & Sharing Rules](adr/ADR-006.md)
   * [ADR-007: Infrastructure Layer & Adapter Rules](adr/ADR-007.md)
   * [ADR-008: ANIMA Core Behaves as a Cognitive Kernel](adr/ADR-008.md)
   * [ADR-009: Configurable AI Model Topology](adr/ADR-009.md)
   * [ADR-010: Adapter–Actuator Split](adr/ADR-010.md)
   * [ADR-011: Event-Based Input Architecture](adr/ADR-011.md)

4. **Documentação Completa de Arquitetura**
   * [ANIMA Architecture](architecture/anima-architecture.md)
   * [Cognitive Kernel](architecture/cognitive-kernel.md)
   * [Seed System](architecture/seed-system.md)
   * [Module Types and Leases](architecture/module-types-and-leases.md)
   * [Event Architecture](architecture/event-architecture.md)
   * [Domain and Infrastructure](architecture/domain-and-infrastructure.md)
   * [AI Model Topology](architecture/ai-model-topology.md)
   * [Adapter-Actuator Split](architecture/adapter-actuator-split.md)
   * [Explicit Semantics and Model Topology](architecture/explicit-semantics-and-model-topology.md)
   * [Directory Overview](architecture/directory-overview.md)

5. **Revisão Completa de Segurança e Governança**
   * [Safety Model](safety/safety-model.md)
   * [Threat Model](safety/threat-model.md)
   * [Memory Integrity](safety/memory-integrity.md)
   * [Licensing Model](governance/licensing-model.md)
   * [Project Viability](governance/project-viability.md)

6. **Especificações**
   * [ANIMA URN Specification](specs/anima-urn.md)
   * [JSON Reference System (JSRS)](specs/json_reference_system.md)

---

## Conceitos-Chave para Entender

Antes de mergulhar profundamente, familiarize-se com estes conceitos fundamentais:

### Engine ≠ Identity
ANIMA separa o engine de raciocínio da identidade. O engine é agnóstico à identidade; personalidades são definidas por **Seeds** (arquivos de configuração declarativos).

### Segurança Acima de Capacidade
ANIMA trata capacidade como risco. Cada ação requer autorização explícita, pode ser inspecionada e deixa uma trilha de auditoria. A segurança é arquitetural, não baseada em prompts.

### Privado por Design
Cada instância ANIMA possui sua própria memória e evolui independentemente. Não há pool de memória compartilhado ou aprendizado entre instâncias.

### Continuidade Acima de Recordação
ANIMA prioriza identidade consistente e continuidade narrativa sobre memória fotográfica perfeita. A memória tem políticas de decaimento intencionais.

### Arquitetura Hexagonal
ANIMA usa arquitetura de portas e adaptadores. O núcleo não tem conhecimento de plataformas, personalidades ou encarnação. Toda E/S acontece através de módulos.

---

## Perguntas Frequentes

Para perguntas frequentes, consulte [FAQ.md](FAQ.md).

Para perguntas sobre o que ANIMA não foi projetado para fazer, consulte [Non-Goals](vision/non-goals.md).

---

## Dicas de Navegação

* **Canonical Glossary** ([foundations/canonical-glossary.md](foundations/canonical-glossary.md)) - Use isto como sua referência para toda terminologia. As definições são autoritativas e nunca devem divergir.

* **ADRs são numerados** - Leia-os em ordem (001-011) para entender a evolução do pensamento arquitetural.

* **Referências cruzadas são intencionais** - Documentos linkam uns aos outros deliberadamente. Siga os links para entender relacionamentos.

* **Seções "Related Documentation"** - A maioria dos documentos inclui essas seções no topo. Use-as para contexto.

---

## O Que ANIMA Não É

Para evitar mal-entendidos sobre o propósito de ANIMA, entenda o que ela explicitamente **não é**:

* Não é um chatbot ou wrapper de IA conversacional
* Não é um agente de propósito geral autônomo
* Não é uma personalidade fixa única
* Não é um sistema de memória compartilhada ou mente coletiva
* Não é um framework de engenharia de prompt
* Não é um sistema RAG (Retrieval-Augmented Generation)

Veja [Why Not an Agent?](vision/why-not-an-agent.md) para explicações detalhadas.

---

## Status da Documentação

ANIMA está atualmente em desenvolvimento ativo. A implementação do engine está em um repositório separado e privado (`engine-core`).

Este repositório de documentação existe para:
* Explicar a arquitetura de forma transparente
* Documentar decisões de design publicamente
* Tornar a visão e o modelo de segurança revisáveis antes do lançamento

Para o status atual de desenvolvimento, consulte [Roadmap](roadmaps/roadmap.md).

---

## Contribuindo para a Documentação

Para orientação sobre propostas de mudanças na documentação, consulte [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md).

---

## Recursos Adicionais

* [Announcements](announcements/) - Anúncios e atualizações do projeto
* [Roadmap](roadmaps/roadmap.md) - Roadmap de desenvolvimento
* [Project Viability](governance/project-viability.md) - Avaliação de sustentabilidade de longo prazo

---

**Última Atualização:** 28 de janeiro de 2026
