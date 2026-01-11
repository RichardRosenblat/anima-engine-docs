# 🧩 Arquitetura ANIMA — Visão Geral Abrangente

**Documentação Relacionada:**
* [Sistema de Seeds](seed-system.md) — Inicialização e separação de identidade
* [Tipos de Módulos e Leases](module-types-and-leases.md) — Ciclo de vida e autorização de módulos
* [Arquitetura de Eventos](event-architecture.md) — Sistema de observabilidade e entrada
* [Kernel Cognitivo](cognitive-kernel.md) — Core como supervisor multitarefa
* [Domínio e Infraestrutura](domain-and-infrastructure.md) — Implementação de arquitetura hexagonal
* [Topologia de Modelos de IA](ai-model-topology.md) — Cortex e Arcuate
* [Divisão Adapter-Actuator](adapter-actuator-split.md) — Estrutura de módulos

**ADRs Relacionados:** Todos os ADRs (ADR-001 até ADR-011)

---

## Introdução

ANIMA é um **motor de IA privado e modular** projetado para hospedar identidades artificiais de longa duração e em evolução sob rigorosas restrições de segurança, memória e capacidade.

A arquitetura segue princípios de **Arquitetura Hexagonal** e **Design Orientado por Domínio (DDD)**, com forte ênfase em:

* Separação de responsabilidades
* Segurança por design
* Comportamento observável e auditável
* Isolamento de identidade
* Extensibilidade modular

---

## Princípios Arquiteturais Centrais

### 1. Motor ≠ Identidade

O Motor ANIMA é agnóstico de identidade. Personalidade e comportamento são introduzidos através de **Seeds** (veja [Sistema de Seeds](seed-system.md)).

* Motor: raciocínio, planejamento, MTL (subsistema de memória), segurança
* Identidade: personalidade, tom, políticas comportamentais (definidas por Seed)

### 2. Arquitetura Hexagonal

> **Domínios nunca devem falar diretamente com o mundo externo.**

* Domínios definem **portas** (interfaces)
* Infraestrutura fornece **adaptadores** (implementações)
* Runtime compõe o sistema

Veja [Domínio e Infraestrutura](domain-and-infrastructure.md) para detalhes.

### 3. Observabilidade e Entrada Baseadas em Eventos

* Toda observabilidade é expressa como **eventos estruturados**
* Todas as entradas são transformadas em **eventos** antes de alcançar o Core
* Sem logs tradicionais, apenas fatos imutáveis

Veja [Arquitetura de Eventos](event-architecture.md) para detalhes.

### 4. Core como Kernel Cognitivo

O Core ANIMA se comporta como um **Kernel Cognitivo**, supervisionando tarefas concorrentes em vez de executá-las diretamente.

Veja [Kernel Cognitivo](cognitive-kernel.md) para detalhes.

### 5. Autorização Baseada em Lease

Toda comunicação Core ↔ Módulo é controlada por **leases criptográficos** sobre **gRPC com mTLS**.

Veja [Tipos de Módulos e Leases](module-types-and-leases.md) para detalhes.

---

## 🌍 MUNDO EXTERNO

```
[ Usuário ]        [ Plataformas / Hardware / APIs ]
```

Sem inteligência aqui. Apenas realidade.

---

## 🟦 CAMADA DE MÓDULOS (Fronteira com Efeitos)

> **Única camada que toca o mundo real**

```
┌───────────────────────────────────────────┐
│               MÓDULOS                     │
│                                           │
│  • Módulo de Entrada Discord              │
│  • Módulo de Entrada CLI                  │
│  • Módulo de Entrada Microfone            │
│                                           │
│  • Módulo de Saída Discord                │
│  • Módulo de Saída CLI                    │
│  • Módulo de Saída TTS                    │
│  • Módulo de Saída Live2D                 │
│                                           │
│  (APIs, hardware, streaming, dispositivos)│
└───────────────────────────────────────────┘
```

📌 **Regras de Módulos:**

* Módulos **capturam** ou **executam**
* Módulos **não pensam**
* Módulos **não decidem**
* Todos os módulos executam **fora de processo** do Core
* Comunicação através de **gRPC com mTLS**

### Tipos de Módulos

Cada módulo declara um de três tipos:

* **Tipo I** — Privado Efêmero (vinculado a lease, Core único)
* **Tipo II** — Privado Residente (longa duração, Core único)
* **Tipo III** — Compartilhado Residente (multi-tenant, governado por infraestrutura)

Veja [Tipos de Módulos e Leases](module-types-and-leases.md) para especificações completas.

---

## 🟨 CAMADA DE ADAPTADORES (Anel de Tradução Pura)

> **Primeiro anel protetor em torno do core**

```
┌───────────────────────────────────────────┐
│               ADAPTADORES                 │
│                                           │
│  Adaptadores de Entrada:                  │
│   • Eventos de plataforma → input.nl      │
│   • Sinais de sistema → input.system      │
│   • Módulo NLP → input.semantic           │
│                                           │
│  Adaptadores de Saída:                    │
│   • Intenção → Comandos de Módulo         │
│   • Validação de schema                   │
│   • Imposição de lease                    │
│                                           │
│  (Puro, determinístico, sem I/O)          │
└───────────────────────────────────────────┘
```

📌 **Regras de Adaptadores:**

* Adaptadores **apenas traduzem**
* Sem efeitos colaterais
* Sem acesso a memória
* Sem permissões
* Executam **fora de processo** do Core
* Validam contra **JSON Schemas**

Veja [Divisão Adapter-Actuator](adapter-actuator-split.md) para detalhes.

---

## 🔄 CAMADA DE ATUADORES (Execução)

> **Execução com efeitos sob controle rigoroso de lease**

Atuadores:

* Executam efeitos colaterais do mundo real (APIs, hardware, plataformas)
* Impõem escopo de lease e políticas de interrupção
* Emitem eventos de observabilidade
* Contêm zero lógica do Core

Veja [Divisão Adapter-Actuator](adapter-actuator-split.md) para detalhes.

---

## 🟩 ANEL DE CAPACIDADES (Poder Declarativo)

> **O que o core tem permissão para querer**

```
┌───────────────────────────────────────────┐
│              CAPACIDADES                  │
│                                           │
│  • send_text                              │
│  • speak_audio                            │
│  • render_avatar                          │
│  • move_robot                             │
│                                           │
│  (Contratos, não implementações)          │
└───────────────────────────────────────────┘
```

📌 **Regras de Capacidades:**

* Capacidades são contratos simbólicos
* Seed + Segurança os controlam
* Não executam nada diretamente
* Declarados via **JSON Schemas**
* Versionados e imutáveis

---

## 🧠 CORE (Kernel Cognitivo)

> **O único lugar onde decisões são tomadas**

```
┌───────────────────────────────────────────┐
│            KERNEL COGNITIVO               │
│                                           │
│  🧩 Cortex (Obrigatório)                  │
│    • Planejamento & Raciocínio            │
│    • Expansão de Tarefas                  │
│    • Seleção de Capacidades               │
│                                           │
│  💬 Arcuate (Opcional)                    │
│    • Processamento NLP                    │
│    • input.nl → input.semantic            │
│    • Intenção → Linguagem Natural         │
│                                           │
│  🎯 Supervisor de Tarefas                 │
│    • Gerenciamento de tarefas concorrentes│
│    • Roteamento de interrupções           │
│    • Ciclo de vida de spans               │
│                                           │
│  Entradas:                                │
│   • Eventos Estruturados                  │
│   • Resultados de Consulta de Memória     │
│   • Restrições de Seed                    │
│   • Políticas de Segurança                │
│                                           │
│  Saída:                                   │
│   • Grafos de Intenção                    │
│   • Diretivas de Tarefas                  │
│   • Eventos de Observabilidade            │
└───────────────────────────────────────────┘
```

📌 **Regras do Core:**

* Core **nunca toca o mundo**
* Core produz **intenção**, não efeitos
* Core **supervisiona** trabalho, não o executa
* Todas as ações são **interruptíveis por design**
* **Cortex** (cognição) é obrigatório
* **Arcuate** (NLP) é opcional

Veja [Kernel Cognitivo](cognitive-kernel.md) e [Topologia de Modelos de IA](ai-model-topology.md) para detalhes.

---

## 🟦 CONTEXTO INTERNO (Influência, Não Controle)

Estes envolvem o core mas **não executam**.

### 🧬 Seed (Identidade Estática)

```
┌───────────────────────────┐
│           SEED            │
│                           │
│  • Parâmetros de          │
│    personalidade          │
│  • Tom / expressividade   │
│  • Tolerância a risco     │
│  • Capacidades permitidas │
│  • Limites de identidade  │
│  • Políticas de memória   │
└───────────────────────────┘
```

* Carregado na inicialização
* Imutável durante execução
* Dados, não código
* Define instância mas não é único à instância

Veja [Sistema de Seeds](seed-system.md) para detalhes.

---

### 🧠 Memória (Dinâmica, Falível)

```
┌───────────────────────────┐
│          MEMÓRIA          │
│                           │
│  • Interações passadas    │
│  • Observações            │
│  • Estados de tarefas     │
│  • Fatos ponderados por   │
│    confiança              │
│  • Apenas local à         │
│    instância              │
└───────────────────────────┘
```

* Local à instância
* Consultada via MTL (subsistema de memória), nunca confiada cegamente
* Em camadas (episódica, semântica, narrativa)
* Sem compartilhamento entre instâncias

---

## 🔐 SEGURANÇA & POLÍTICA (Transversal)

```
┌───────────────────────────┐
│         SEGURANÇA         │
│                           │
│  • Autenticação           │
│  • Autorização            │
│  • Gerenciamento de Lease │
│  • Imposição de permissões│
│  • Controles de ações     │
│    perigosas              │
│  • Registro de auditoria  │
└───────────────────────────┘
```

Segurança:

* Envolve **entrada antes do core**
* Valida **intenção antes da execução**
* Emite e gerencia **leases criptográficos**
* Impõe **limites de capacidade**

---

## 🔁 FLUXO COMPLETO (LIMPO & LINEAR)

```
Usuário / Ambiente
 ↓
Módulo de Entrada (captura)
 ↓
Adaptador (traduz para eventos)
 ↓
Eventos: input.nl / input.system / input.semantic
 ↓
Autenticação / Segurança
 ↓
KERNEL COGNITIVO
  ↔ Memória (consulta)
  ↔ Seed (restrições)
  ↔ Cortex (raciocínio)
  ↔ Arcuate (NLP opcional)
 ↓
Intenção + URN de Capacidade
 ↓
Segurança (valida lease & permissões)
 ↓
Adaptador de Saída (valida schema, mapeia para comando)
 ↓
Atuador (executa sob lease)
 ↓
Efeito + Eventos de Observabilidade
```

Sem atalhos. Sem vazamentos. Todos os passos observáveis e auditáveis.

---

## Arquitetura Baseada em Eventos

Toda atividade do sistema é expressa através de **eventos estruturados**:

* **input.nl** — Linguagem natural (pré-semântica)
* **input.system** — Fatos de sistema/mecânicos
* **input.semantic** — Significado afirmado pós-interpretação
* **capability.invoked** — Execução de capacidade iniciada
* **capability.completed** — Execução de capacidade finalizada
* **capability.interrupted** — Execução de capacidade interrompida

Eventos são:
* Imutáveis
* Com escopo de execução
* Rastreáveis causalmente
* Seguros para concorrência

Veja [Arquitetura de Eventos](event-architecture.md) para detalhes.

---

## Interrupção & Multitarefa

O Core opera como um **kernel multitarefa** que suporta:

* Execução concorrente de tarefas
* Interrupção cooperativa
* Gerenciamento explícito de spans
* Preempção governada por política

Interrupções são:
* Eventos de primeira classe
* Classificadas (override/cancel/queue/clarification/emergency)
* Governadas por política
* Observáveis e auditáveis

Veja [Kernel Cognitivo](cognitive-kernel.md) para detalhes.

---

## Design Orientado por Domínio

ANIMA segue regras rigorosas de **Arquitetura Hexagonal**:

```
infra  ─────▶  domínios
runtime ────▶  infra + domínios

domínios ─╳──▶  infra
```

* Domínios definem **portas** (interfaces)
* Infraestrutura implementa **adaptadores**
* Domínios contêm **apenas lógica de negócio**
* Runtime monta o sistema

Veja [Domínio e Infraestrutura](domain-and-infrastructure.md) para detalhes.

---

## 🧠 Teste de Litmus Arquitetural

Pergunte:

* Posso simular tudo sem módulos? → **Sim**
* Posso trocar Discord por Slack sem tocar o core? → **Sim**
* Posso executar múltiplos Seeds no mesmo motor? → **Sim**
* Posso auditar intenção antes da execução? → **Sim**
* Interrupções podem ser reproduzidas de logs de eventos? → **Sim**
* Módulos podem executar sem código do Core carregado? → **Sim**
* Toda execução é rastreável e observável? → **Sim**

---

## Invariantes Arquiteturais Principais

Os seguintes são **restrições arquiteturais não-negociáveis**:

1. **Motor é agnóstico de identidade** — personalidade vem de Seeds
2. **Core nunca carrega código de terceiros** — módulos executam fora de processo
3. **Toda execução requer leases válidos** — sem lease, sem execução
4. **Domínios nunca importam infraestrutura** — apenas portas/interfaces
5. **Toda observabilidade é baseada em eventos** — sem logs tradicionais
6. **Memória é estritamente com escopo de instância** — sem compartilhamento entre instâncias
7. **Cortex é obrigatório, Arcuate é opcional** — cognição vs. linguagem
8. **Core supervisiona, não executa** — modelo de kernel
9. **Todas as ações são interruptíveis** — interrupção cooperativa
10. **Eventos são a fonte da verdade** — fatos imutáveis e auditáveis

---

## Resumo

ANIMA é um **kernel cognitivo** que:

* Separa motor de identidade
* Supervisiona comportamento em vez de executá-lo
* Comunica através de eventos estruturados
* Impõe segurança através de leases criptográficos
* Mantém limites arquiteturais limpos
* Habilita identidades de longa duração e em evolução

> **ANIMA não executa instruções. Ela supervisiona comportamento.**
> **ANIMA não registra texto. Ela registra fatos.**
> **ANIMA é um motor para identidades crescentes, não uma personalidade única.**
