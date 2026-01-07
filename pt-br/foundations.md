# Fundamentos da ANIMA

Este documento fornece navegação para os três documentos fundamentais que definem a lei constitucional, o vocabulário canônico e os limites arquiteturais da ANIMA.

---

## Documentos Fundamentais

### [1️⃣ Carta do Projeto](project-charter.md)

**Propósito:** Define *"o que existe"* e *"o que é proibido"*.

**Conteúdo:**
* O que é ANIMA (e o que não é)
* Propósito central e valores
* Não-objetivos explícitos
* Invariantes arquiteturais
* Critérios de sucesso
* Modelo de governança

**Princípio Fundamental:** Cada funcionalidade deve ser justificável contra esta carta.

**Leia:** [Carta do Projeto →](project-charter.md)

---

### [2️⃣ Glossário Canônico](canonical-glossary.md)

**Propósito:** Estabelece o vocabulário canônico da ANIMA.

**Conteúdo:**
* Componentes do sistema (Engine, Core, Seed, Memory, etc.)
* Conceitos arquiteturais (Module, Adapter, Actuator, Capability, etc.)
* Modelos de IA (Cortex, Arcuate)
* Conceitos de observabilidade (Event, Execution, Lease, etc.)
* Processo e estado (Zero-Lease State, Task Recovery Grace Period)

**Princípio Fundamental:** Essas definições **nunca devem divergir**.

**Leia:** [Glossário Canônico →](canonical-glossary.md)

---

### [3️⃣ Limites do Sistema](system-boundaries.md)

**Propósito:** Define o que ANIMA pode e não pode fazer por design.

**Conteúdo:**
* O que o motor NUNCA pode fazer (10 invariantes arquiteturais)
* O que SEMPRE DEVE exigir confirmação (5 categorias)
* O que é delegado aos módulos (5 responsabilidades)
* Limites de segurança
* Modos de falha e limites

**Princípio Fundamental:** Limites são impostos pela arquitetura, não por convenção. Violações são bugs.

**Leia:** [Limites do Sistema →](system-boundaries.md)

---

## Relação com a Arquitetura

Estes documentos fundamentais trabalham junto com a [Documentação de Arquitetura](architecture/README.md) para definir completamente a ANIMA:

* **Fundamentos** (este) → Princípios constitucionais e vocabulário
* **Arquitetura** → Implementação detalhada e padrões de design
* **ADRs** → Registros formais de decisão explicando o porquê

---

## Referência Rápida

### Valores Centrais

1. **Verdade sobre confiança** — Admitir incerteza em vez de fabricar
2. **Intenção sobre execução** — Produzir intenção, não efeitos diretos
3. **Modularidade sobre monólito** — Componentes intercambiáveis
4. **Segurança sobre capacidade** — Permissões primeiro, funcionalidades depois
5. **Configurabilidade sobre hardcoding** — Comportamento orientado por dados (Seeds)
6. **Isolamento sobre conveniência** — Segurança através da separação

### Invariantes Arquiteturais

1. O motor é agnóstico de identidade (personalidade vem de Seeds)
2. O Core nunca carrega código de terceiros (módulos fora de processo)
3. Toda execução requer leases válidos
4. Domínios nunca importam infraestrutura (arquitetura hexagonal)
5. Toda observabilidade é baseada em eventos (sem logs tradicionais)
6. Memória é estritamente escopo de instância
7. Cortex é obrigatório, Arcuate é opcional
8. Core supervisiona, não executa (kernel cognitivo)
9. Todas as ações são interruptíveis (interrupção cooperativa)
10. Eventos são a fonte da verdade (imutáveis, auditáveis)

### Limites Principais

* **Core nunca toca o mundo** — Intenção, não efeitos
* **Sem lease, sem execução** — Zero-Lease State
* **Sem memória entre instâncias** — Apenas local à instância
* **Sem auto-modificação** — Código e Seeds estáveis
* **Ações perigosas requerem confirmação** — Aprovação explícita do usuário

---

## Resumo

> **ANIMA é um motor para identidades crescentes, não uma personalidade única.**
>
> **Estes fundamentos são constitucionais. Eles definem o que ANIMA é e nunca pode se tornar.**
>
> **Precisão na linguagem previne confusão na arquitetura.**
