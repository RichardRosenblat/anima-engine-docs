# Documentação do ANIMA Engine

![Status](https://img.shields.io/badge/status-desenvolvimento%20ativo-blue)
![Architecture](https://img.shields.io/badge/architecture-hexagonal%20%2F%20ports--and--adapters-purple)

**ANIMA** é um motor de IA privado e modular projetado para hospedar identidades artificiais de longa duração e em evolução, sob restrições rígidas de segurança, memória e capacidades.

Não é um chatbot, não é um wrapper de prompt e não é uma personalidade única.

ANIMA é um **motor para cultivar identidades**.

---

## Começando

**Novo no ANIMA?** Comece aqui:

* **[Guia de Início](GETTING_STARTED.md)** - Navegue pela documentação de forma eficiente com ordens de leitura sugeridas para diferentes públicos
* **[FAQ](FAQ.md)** - Perguntas comuns e respostas rápidas
* **[Visão](vision/vision.md)** - Entenda o que o ANIMA está tentando possibilitar

---

## Princípios Fundamentais

* **Motor ≠ Identidade**
  Raciocínio e execução são estritamente separados de personalidade e comportamento.

* **Privado por Design**
  Cada instância ANIMA possui sua própria memória e evolui independentemente.

* **Segurança sobre Capacidade**
  Permissões, confirmações e limites são aplicados no nível do motor.

* **Modularidade em Primeiro Lugar**
  Entradas, saídas, capacidades e incorporação são conectáveis.

* **Continuidade sobre Recordação**
  ANIMA prioriza identidade consistente e confiança sobre memória perfeita.

---

## Visão Geral da Arquitetura

ANIMA segue uma arquitetura hexagonal / portas-e-adaptadores.

```
┌────────────────────┐
│  Módulos de Entrada │  (texto, voz, discord, sensores)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│   Motor ANIMA      │
│                    │
│  - Raciocínio      │
│  - Planejamento    │
│  - Sistema de      │
│    Tarefas         │
│  - Camadas de      │
│    Memória         │
│  - Segurança       │
│  - Capacidades     │
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│    Adaptadores     │  (Conversão de intenção para comando)
└────────────────────┘
          │
┌─────────▼──────────┐
│ Módulos de         │  (ferramentas, streaming, robôs, etc)
│ Capacidade         │
└────────────────────┘
```

O motor **não tem conhecimento** de plataformas, personalidades ou incorporação.

---

## O Sistema de Seeds

Um **Seed** define uma identidade ANIMA.

Um seed é **dados**, não código.

### Um seed pode definir:

* Parâmetros de personalidade (tom, verbosidade, expressividade emocional)
* Políticas comportamentais (tolerância a riscos, tratamento de incertezas)
* Listas de permissão/negação de capacidades
* Estilo de interação padrão
* Enquadramento narrativo inicial
* Regras de decaimento e promoção de memória

### Um seed não deve incluir:

* Memórias aprendidas
* Logs de conversação
* Dados de usuário
* Lógica de execução
* Estado entre instâncias

Após a inicialização, cada ANIMA Identity desenvolve **memória local da identidade** que evolui independentemente.

Uma **ANIMA Identity** é composta por: **Seed** (priors de identidade) + **Memory** (experiências em evolução).

Uma **ANIMA Instance** é uma execução em andamento que hospeda uma ANIMA Identity.

**Sentença Canônica:** "Uma ANIMA Instance executa uma ANIMA Identity derivada de um ANIMA Seed e Memórias."

---

## ANIMA Prime Identity vs Identidades de Usuário

* **ANIMA Prime Identity**
  Uma identidade especial e protegida projetada para o Motor ANIMA.
  Usada exclusivamente para incorporação em streaming / VTuber e como implementação de referência.
  Implementada como:

  ```
  Motor ANIMA + Prime Seed + Prime Memory
  ```

  **Restrições Críticas:**
  * Prime Seed NUNCA é distribuído ou compartilhado
  * Prime Memory NUNCA é exportada ou compartilhada
  * Prime Identity não pode ser instanciada por terceiros
  * Prime Identity é não-clonável e não-exportável

* **Identidades ANIMA de Usuário**
  Identidades privadas inicializadas com seeds selecionados pelo usuário.
  Os usuários são proprietários da memória e evolução de sua Identity.
  Cada Identity pode ser carregada em uma ANIMA Instance.

Todas as Identities executam no **mesmo motor**, mas permanecem totalmente isoladas.

**Nota:** Uma ANIMA Instance (execução em andamento) hospeda uma ANIMA Identity (Seed + Memory).

---

## Modelo de Memória

ANIMA usa memória em camadas para equilibrar realismo, segurança e custo.

Tipos de memória, políticas de promoção e decaimento especificadas no [documento de integridade de memória](safety/memory-integrity.md)

ANIMA rastreia se o conhecimento é:

* observado
* lembrado
* inferido
* desconhecido

Ela nunca supõe silenciosamente.

---

## Capacidades

Capacidades são módulos explícitos e descobríveis.

O motor:

* raciocina sobre capacidades
* verifica permissões
* solicita confirmação quando necessário
* executa em uma sandbox controlada

Seeds podem **restringir** capacidades, mas nunca conceder novo poder de execução.

---

## Segurança & Permissões

* Identidades de usuário estáveis
* Acesso baseado em função
* Verificações de permissão em nível de capacidade
* Confirmação humana para ações perigosas
* Registro de auditoria completo

ANIMA explica *por que* ela pode ou não pode agir.

---

## O que ANIMA Não É

* ❌ Um agente autônomo de propósito geral
* ❌ Uma IA conectada à internet por padrão
* ❌ Um sistema de memória compartilhada
* ❌ Uma personalidade codificada em prompts
* ❌ Uma substituição para o julgamento humano

---

## Status do Projeto

ANIMA está em desenvolvimento ativo.

Foco atual:

* Correção do motor central
* Integridade da memória
* Limites de capacidades
* Validação de seeds

Este projeto está evoluindo intencionalmente de forma lenta.

---

## Documentos de Design

Decisões arquiteturais importantes são rastreadas como ADRs em [`/adr`](adr):

* ADR-001: Separação do Motor / Identidade através do Sistema de Seeds
* ADR-002: Esquemas Auto-Validáveis com Restrições Somente Internas
* ADR-003: Protocolo de Comunicação Core ↔ Módulo, Tipos de Módulo e Ciclo de Vida de Lease
* ADR-004: Observabilidade, Registro de Eventos e Rastreabilidade de Execução
* ADR-005: Modelo de Interrupção e Preempção
* ADR-006: Regras de Dependência e Compartilhamento de Domínio
* ADR-007: Camada de Infraestrutura e Regras de Adaptador
* ADR-008: ANIMA Core se Comporta como um Kernel Cognitivo
* ADR-009: Topologia de Modelo de IA Configurável com Córtex Obrigatório e Arcuate Opcional
* ADR-010: Divisão Adaptador–Atuador com Fronteira de Processo Estrita
* ADR-011: Arquitetura de Entrada Baseada em Eventos

### Documentação de Arquitetura

Documentação abrangente de arquitetura derivada dos ADRs:

* [Visão Geral da Arquitetura](architecture/directory-overview.md) - Guia de navegação completo
* [Arquitetura ANIMA](architecture/anima-architecture.md) - Visão geral abrangente do sistema
* [Sistema de Seeds](architecture/seed-system.md) - Inicialização e separação de identidade
* [Tipos de Módulo e Leases](architecture/module-types-and-leases.md) - Ciclo de vida e autorização de módulos
* [Arquitetura de Eventos](architecture/event-architecture.md) - Observabilidade e sistema de entrada
* [Kernel Cognitivo](architecture/cognitive-kernel.md) - Core como supervisor multitarefa
* [Domínio e Infraestrutura](architecture/domain-and-infrastructure.md) - Implementação de arquitetura hexagonal
* [Topologia do Modelo de IA](architecture/ai-model-topology.md) - Córtex e Arcuate
* [Divisão Adaptador-Atuador](architecture/adapter-actuator-split.md) - Estrutura de módulo

### Documentos Fundamentais

Princípios constitucionais e limites:

* [Glossário Canônico](foundations/canonical-glossary.md) - Terminologia e conceitos definitivos
* [Limites do Sistema](foundations/system-boundaries.md) - O que ANIMA pode e não pode fazer

### Documentação Adicional

* [Visão](vision/vision.md) - Visão e objetivos de longo prazo
* [Carta do Projeto](vision/project-charter.md) - Propósito central, valores e não-objetivos
* [Não-Objetivos](vision/non-goals.md) - Não-objetivos e limites explícitos
* [Integridade da Memória](safety/memory-integrity.md) - Gerenciamento e integridade da memória
* [Modelo de Segurança](safety/safety-model.md) - Arquitetura de segurança
* [Modelo de Ameaças](safety/threat-model.md) - Análise e mitigação de ameaças
* [Modelo de Licenciamento](governance/licensing-model.md) - Licenciamento e distribuição
* [Roadmap](roadmaps/roadmap.md) - Roteiro de desenvolvimento
* [Anúncios](announcements) - Anúncios e atualizações do projeto
* [Especificações](specs) - Especificações técnicas

---

## Filosofia

ANIMA não é projetada para parecer inteligente a todo custo.

Ela é projetada para parecer **consistente, honesta e segura para crescer junto**.

---

## Perguntas Frequentes

**Para o FAQ completo, consulte [FAQ.md](FAQ.md)**

### O que é ANIMA, exatamente?

ANIMA é um **motor de IA privado e modular** projetado para hospedar identidades artificiais de longa duração e em evolução sob restrições estritas de segurança, memória e capacidades.

Ela **não é**:
* um chatbot
* uma personalidade única
* um agente autônomo que age sozinho

ANIMA é um *motor para cultivar identidades*, não um personagem pré-embalado.

### Onde posso encontrar o motor ANIMA?

O motor ANIMA está em um repositório separado chamado `engine-core`, que está atualmente **não público** enquanto os limites centrais de segurança, memória e identidade estão em desenvolvimento.

Este repositório de documentação pública existe para explicar a arquitetura, documentar decisões de design e tornar a visão de longo prazo transparente.

### Como faço para baixar ou executar ANIMA?

ANIMA **ainda não está disponível para download**. Quando lançado, será distribuído como um runtime de motor onde os usuários inicializam suas próprias instâncias privadas usando Seeds.

### O que é um "Seed"?

Um Seed é um **blueprint de identidade estruturado** usado na inicialização. Ele define parâmetros de personalidade, restrições comportamentais, capacidades permitidas e enquadramento narrativo inicial—mas não inclui memórias, histórico de conversas ou comportamento aprendido.

### ANIMA é segura para usar?

Segurança é um **requisito arquitetural central**. ANIMA impõe permissões explícitas, listas de permissão/negação de capacidades, confirmação humana para ações perigosas, registro de auditoria e separação estrita entre identidade e execução.

### Mais Perguntas?

Consulte o [FAQ](FAQ.md) completo para respostas detalhadas a perguntas comuns sobre arquitetura, segurança, privacidade e desenvolvimento.

---

### Como posso acompanhar o desenvolvimento?

Atualizações, decisões arquiteturais e filosofia estão documentadas neste repositório.

Marcos importantes serão anunciados assim que o motor atingir fases estáveis.

Anúncios serão adicionados na pasta `/announcements` e compartilhados através de canais oficiais futuros.

---

## Contribuindo para a Documentação

Interessado em melhorar a documentação do ANIMA? Consulte [DOCUMENTATION_MAINTENANCE.md](DOCUMENTATION_MAINTENANCE.md) para:

* Como propor mudanças na documentação
* Quando criar novos ADRs vs. atualizar existentes
* Diretrizes de revisão da documentação
* Diretrizes de estilo e tom

---

**Veja também**: [Documentação completa em Inglês](../README.md)
