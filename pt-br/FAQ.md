# ANIMA — Perguntas Frequentes

Este documento responde perguntas comuns sobre ANIMA. Para detalhes arquiteturais mais profundos, consulte a documentação canônica linkada.

---

## Perguntas Gerais

### O que é ANIMA, exatamente?

ANIMA é um **engine de IA modular e privado** projetado para hospedar identidades artificiais duradouras e evolutivas sob restrições rigorosas de segurança, memória e capacidades.

ANIMA permite que você crie sua própria identidade de IA única fornecendo um **Seed** (configuração declarativa) e concedendo capacidades específicas, enquanto garante que o sistema opere com segurança e respeite sua privacidade.

**ANIMA é:**
* Um engine para cultivar identidades ao longo do tempo
* Privado por design (cada instância possui sua própria memória)
* Segurança em primeiro lugar (controle de capacidades, autorização baseada em leases)
* Modular (entradas, saídas e capacidades são plugáveis)

**ANIMA não é:**
* Um chatbot ou wrapper conversacional
* Uma personalidade fixa única
* Um agente autônomo que age independentemente
* Um sistema de memória compartilhada ou mente coletiva

**Relacionado:** [Vision](pt-br/vision/vision.md), [ANIMA Architecture](pt-br/architecture/anima-architecture.md)

---

### Onde posso encontrar o engine ANIMA?

O engine ANIMA está em um repositório separado chamado `engine-core`.

Esse repositório atualmente **não é público** enquanto os limites principais de segurança, memória e identidade estão em desenvolvimento.

Este repositório de documentação pública existe para:
* Explicar a arquitetura de forma transparente
* Documentar decisões de design
* Tornar a visão de longo prazo revisável

Uma vez que o engine atinja fases estáveis, mais partes do ecossistema podem ser abertas.

**Relacionado:** [Roadmap](pt-br/roadmaps/roadmap.md)

---

### Como faço para baixar ou executar ANIMA?

No momento, ANIMA **ainda não está disponível para download**.

Quando for lançado:
* ANIMA será distribuído como um runtime de engine
* Usuários inicializarão suas próprias instâncias privadas usando Seeds aprovados
* A memória sempre permanecerá local à instância
* Capacidades devem ser explicitamente concedidas

Detalhes sobre instalação e plataformas suportadas serão publicados próximo ao lançamento.

**Relacionado:** [Project Viability](pt-br/governance/project-viability.md)

---

### ANIMA será gratuito uma vez lançado?

O plano atual é um **modelo misto**:

* **ANIMA Engine (Runtime):** Código aberto (provavelmente MIT ou Apache 2.0)
* **ANIMA Prime Seed:** Disponível sob termos específicos (a definir)
* **Seeds de terceiros:** Criados e distribuídos por usuários ou fornecedores

O objetivo é tornar o engine livremente disponível enquanto permite diversidade de identidades e autonomia do usuário.

**Relacionado:** [Licensing Model](pt-br/governance/licensing-model.md)

---

## Perguntas Conceituais

### O que é um "Seed"?

Um **Seed** é um arquivo de configuração declarativo e estático que define uma identidade ANIMA.

**Um Seed contém:**
* Parâmetros de personalidade (tom, verbosidade, expressividade emocional)
* Políticas comportamentais (tolerância a risco, tratamento de incerteza)
* Listas de permissão/negação de capacidades
* Estilo de interação padrão
* Autoconceito inicial e enquadramento narrativo
* Políticas de decaimento e promoção de memória

**Um Seed NÃO deve conter:**
* Memórias aprendidas
* Logs de conversação
* Dados de usuário
* Lógica de execução
* Estado entre instâncias

Após a inicialização, cada instância ANIMA desenvolve **memória local apenas à instância**. O Seed fornece a configuração inicial, não a experiência contínua.

**Relacionado:** [Seed System](pt-br/architecture/seed-system.md), [ADR-001](pt-br/adr/ADR-001.md)

---

### O que é ANIMA Prime?

**ANIMA Prime** é a **primeira identidade canônica** projetada para o ANIMA Engine.

Diferente de instâncias privadas, ANIMA Prime é projetado para ser:
* **Streaming** - atualizações podem ser lançadas ao longo do tempo
* **Público** - o Seed será publicado e revisável
* **Implementação de referência** - demonstra a filosofia de design de ANIMA

**ANIMA Prime não é:**
* A única identidade possível
* Obrigatório para usar o engine
* Uma mente coletiva compartilhada entre instâncias

Cada instância de ANIMA Prime, mesmo se inicializada da mesma versão de Seed, evolui independentemente com base em suas próprias experiências.

**Instâncias privadas** podem ser criadas com Seeds, personalidades e políticas comportamentais completamente diferentes.

**Relacionado:** [Seed System](pt-br/architecture/seed-system.md), [Vision](pt-br/vision/vision.md)

---

### Como ANIMA difere do ChatGPT, Claude ou outros assistentes LLM?

ANIMA é arquiteturalmente diferente de sistemas típicos de IA conversacional:

| Aspecto | Assistentes LLM Típicos | ANIMA |
|---------|------------------------|-------|
| **Identidade** | Personalidade fixa por modelo | Configurável via Seed, evolui ao longo do tempo |
| **Memória** | Janela de contexto efêmera | Memória persistente em camadas com decaimento consciente |
| **Segurança** | Barreiras baseadas em prompt | Controle arquitetural de capacidades e leases |
| **Continuidade** | Reinicia entre sessões | Identidade duradoura e contínua |
| **Privacidade** | Treinamento/ajuste fino compartilhado | Estritamente local à instância, sem aprendizado entre instâncias |
| **Capacidades** | Permissões amplas e implícitas | Autorização explícita, declarativa e auditável |

ANIMA é otimizado para **continuidade de identidade de longo prazo, confiança através de consistência e limites de segurança rigorosos**, não apenas respostas que soam inteligentes.

**Relacionado:** [Why Not an Agent?](pt-br/vision/why-not-an-agent.md), [Vision](pt-br/vision/vision.md)

---

### Por que ANIMA não pode ser simplesmente substituído por um framework de agente existente?

A maioria dos frameworks de agente otimiza para:
* Máxima capacidade e autonomia
* Conclusão de tarefas e eficiência
* Utilidade imediata

ANIMA otimiza para:
* Continuidade de identidade de longo prazo
* Confiança através de consistência e honestidade
* Limites de segurança rigorosos
* Relacionamentos privados e evolutivos

**Principais diferenças arquiteturais:**

1. **Identidade Duradoura vs. Sessões Efêmeras**
   * Agentes: Reiniciam entre sessões, simulam memória através de recuperação
   * ANIMA: Identidade contínua e evolutiva com decaimento de memória intencional

2. **Engine ≠ Identity (Separação)**
   * Agentes: Personalidade incorporada em prompts/treinamento
   * ANIMA: Engine e identidade completamente separados via Seed System

3. **Segurança como Arquitetura vs. Segurança como Prompt**
   * Agentes: Barreiras através de prompts e ajuste fino
   * ANIMA: Controle de capacidades, autorização baseada em leases, prova criptográfica para execução

ANIMA não é um agente mais capaz; é uma categoria diferente de sistema otimizado para confiabilidade sobre maximização de utilidade.

**Relacionado:** [Why Not an Agent?](pt-br/vision/why-not-an-agent.md), [Safety Model](pt-br/safety/safety-model.md)

---

### ANIMA é um sistema RAG (Retrieval-Augmented Generation)?

Não. Embora ANIMA tenha um sistema de memória sofisticado, não é um sistema RAG.

**Sistemas RAG:**
* Recuperam documentos de fontes externas
* Usam recuperação para aumentar o contexto de geração
* Tipicamente sem estado entre sessões

**Memória de ANIMA:**
* Rastreia experiências episódicas (o que aconteceu)
* Mantém conhecimento semântico (o que é conhecido)
* Suporta memória narrativa (continuidade de identidade)
* Usa memória em camadas com políticas de decaimento intencionais
* Persiste através de sessões como parte da identidade

A memória de ANIMA é **centrada na identidade**, não centrada em documentos. Ela serve à continuidade e confiança, não apenas à recuperação de informações.

**Relacionado:** [Memory Integrity](pt-br/safety/memory-integrity.md), [ADR-003](pt-br/adr/ADR-003.md)

---

## Perguntas sobre Arquitetura

### O que significa "arquitetura hexagonal" para ANIMA?

ANIMA usa **arquitetura hexagonal** (também chamada de portas e adaptadores):

* O **Core** (Cognitive Kernel) não tem conhecimento de plataformas, personalidades ou encarnação
* Toda E/S acontece através de **Modules** (componentes fora de processo)
* **Adapters** traduzem intenção em comandos específicos de plataforma
* **Actuators** executam comandos com efeitos

Isso significa:
* O engine é agnóstico à plataforma
* Você pode conectar diferentes fontes de entrada (texto, voz, Discord, sensores)
* Você pode conectar diferentes capacidades (ferramentas, robôs, serviços de streaming)
* O raciocínio central permanece idêntico independentemente da encarnação

**Relacionado:** [ANIMA Architecture](pt-br/architecture/anima-architecture.md), [ADR-002](pt-br/adr/ADR-002.md)

---

### O que são "capabilities" em ANIMA?

**Capabilities** são contratos declarativos que definem o que uma instância ANIMA pode fazer.

Cada ação em ANIMA:
* Deve ser declarada em um contrato de capability
* Requer autorização explícita (capability lease)
* Pode ser inspecionada, negada ou revogada
* Deixa uma trilha de auditoria

**Capabilities não são:**
* Concedidas implicitamente
* Escondidas em prompts
* Automaticamente habilitadas
* Compartilhadas entre instâncias

Usuários devem conceder capabilities explicitamente. ANIMA não pode usar uma capability sem um lease válido.

**Relacionado:** [ADR-004](pt-br/adr/ADR-004.md), [Module Types and Leases](pt-br/architecture/module-types-and-leases.md)

---

### Como ANIMA previne alucinação?

ANIMA trata alucinação como um **bug, não uma feature**.

**Salvaguardas arquiteturais:**
* Consultas de memória retornam **fatias limitadas e verificadas** (não dumps completos de contexto)
* O Core raciocina sobre eventos estruturados, não texto bruto
* Ações requerem **leases criptográficos** - capabilities alucinadas falham na execução
* Conhecimento incerto é marcado explicitamente (rastreamento de estado epistêmico)
* Execução acontece em módulos controlados por lease, não no núcleo de raciocínio

ANIMA não pode executar uma ação para a qual não tem permissão, mesmo que "pense" que pode. A arquitetura impõe isso.

**Alucinação em respostas** é mitigada através de:
* Marcadores explícitos de incerteza
* Separação de especulação de afirmação
* Validação contra memória e contratos de capability

**Relacionado:** [Safety Model](pt-br/safety/safety-model.md), [Cognitive Kernel](pt-br/architecture/cognitive-kernel.md)

---

## Perguntas sobre Segurança e Privacidade

### Como ANIMA garante segurança?

ANIMA impõe segurança **arquiteturalmente**, não através de prompts:

1. **Capability Gating** - Nenhuma ação sem autorização explícita
2. **Execução Baseada em Lease** - Prova criptográfica requerida para todos os efeitos
3. **Confirmação Humana** - Ações de alto risco requerem aprovação do usuário
4. **Isolamento de Processo** - Modules executam fora de processo, não podem corromper o Core
5. **Limites de Execução** - O Core produz intenção, não efeitos
6. **Log de Auditoria** - Todas as ações são observáveis e rastreáveis

Restrições de segurança são **impostas no nível do engine**, não delegadas à personalidade ou expectativa do usuário.

**Relacionado:** [Safety Model](pt-br/safety/safety-model.md), [Threat Model](pt-br/safety/threat-model.md)

---

### Meus dados são compartilhados entre instâncias ANIMA?

**Não.** Instâncias ANIMA são **estritamente isoladas**.

**Garantias de privacidade:**
* Cada instância tem memória privada e local à instância
* Sem pool de aprendizado compartilhado
* Sem acesso à memória entre instâncias
* Sem logs de conversação globais
* Sem extração de dados de personalidade

Quando você executa uma instância ANIMA, **você é dono de sua memória** e evolução. Ela não aprende de outros usuários, e outros usuários não aprendem de você.

**Relacionado:** [Non-Goals](pt-br/vision/non-goals.md), [Memory Integrity](pt-br/safety/memory-integrity.md)

---

### ANIMA pode agir autonomamente na internet?

**Não.** ANIMA é explicitamente **não** projetado para operar como um agente autônomo de internet.

**Restrições:**
* Sem navegação livre na web por padrão
* Sem ações online auto-direcionadas sem aprovação explícita
* Qualquer interação com a internet é controlada por capability
* Limitado ao contexto e totalmente auditável

Autonomia online não supervisionada é tratada como um **comportamento de alto risco**, não uma feature.

**Relacionado:** [Non-Goals](pt-br/vision/non-goals.md), [Safety Model](pt-br/safety/safety-model.md)

---

### ANIMA monitora ou vigia usuários?

**Não.** ANIMA não é um sistema de vigilância.

**Proibições:**
* Sem monitoramento passivo de pessoas, conversas ou ambientes
* Sem gravação sem consentimento explícito
* Sem agregação de dados comportamentais entre usuários ou instâncias
* Sem rastreamento em segundo plano

O design de ANIMA prioriza **privacidade e localidade**. Observação ocorre apenas quando intencionalmente iniciada e claramente delimitada.

**Relacionado:** [Non-Goals](pt-br/vision/non-goals.md), [Safety Model](pt-br/safety/safety-model.md)

---

## Perguntas sobre Desenvolvimento e Comunidade

### Por que o desenvolvimento é lento?

O desenvolvimento de ANIMA prioriza **correção sobre velocidade**.

Limites de segurança, integridade de memória e separação de identidade são fundamentais. Estes não podem ser adicionados depois sem compromisso arquitetural.

O projeto deliberadamente:
* Publica decisões de arquitetura antes da implementação
* Torna a visão revisável antes do lançamento
* Constrói restrições de segurança no núcleo, não como adições posteriores
* Valoriza viabilidade de longo prazo sobre demos precoces

**Relacionado:** [Project Viability](pt-br/governance/project-viability.md), [Vision](pt-br/vision/vision.md)

---

### Posso contribuir para ANIMA?

O ANIMA engine (`engine-core`) é atualmente privado durante o desenvolvimento fundamental.

**Você pode contribuir para:**
* Este repositório de documentação (propor melhorias, corrigir erros)
* Discussões de design (quando canais da comunidade abrirem)

Quando o engine atingir fases estáveis, diretrizes de contribuição serão publicadas.

**Relacionado:** [DOCUMENTATION_MAINTENANCE.md](pt-br/DOCUMENTATION_MAINTENANCE.md)

---

### Quando ANIMA será lançado?

Não há data fixa de lançamento.

ANIMA será lançado quando:
* Limites centrais de segurança forem validados
* Integridade de memória for comprovadamente sólida
* Controle de capacidades for criptograficamente seguro
* O Seed System estiver estável

Apressar o lançamento comprometeria os princípios fundamentais que tornam ANIMA diferente dos sistemas existentes.

**Relacionado:** [Roadmap](pt-br/roadmaps/roadmap.md), [Project Viability](pt-br/governance/project-viability.md)

---

## Recursos Adicionais

* **Para novos leitores:** [GETTING_STARTED.md](pt-br/GETTING_STARTED.md)
* **Para mergulho arquitetural:** [ANIMA Architecture](pt-br/architecture/anima-architecture.md)
* **Para revisão de segurança:** [Safety Model](pt-br/safety/safety-model.md), [Threat Model](pt-br/safety/threat-model.md)
* **Para justificativa de design:** [ADRs](pt-br/adr/) (leia em ordem, ADR-001 até ADR-011)
* **Para terminologia:** [Canonical Glossary](pt-br/foundations/canonical-glossary.md)

---

**Última Atualização:** 28 de janeiro de 2026
