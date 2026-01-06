# ANIMA — Roteiro de Desenvolvimento

**Versão:** 1.0
**Escopo:** Motor, Sistema de Seed, Instâncias, Produtização
**Princípio Orientador:** *Motor ≠ Identidade ≠ Memória*
**Fase Atual:** Fase 1 — Esqueleto do Motor Central (Livre de Identidade)

---

## Fase 0 — Fundamentos (Não Pule)

### 🎯 Objetivo

Definir o que a ANIMA *é* e *não é*.

### 🧱 Construir

1. **Carta do Projeto**

   * Propósito central (motor de IA privado e evolutivo)
   * Não-objetivos (sem autonomia descontrolada, sem internet por padrão)
   * Valores centrais (verdade sobre confiança, segurança sobre capacidade)

2. **Glossário**

   * Motor
   * Seed
   * Instância
   * Memória
   * Capacidade
   * Adaptador

3. **Limites do Sistema**

   * O que o motor *nunca* pode fazer
   * O que deve *sempre* exigir confirmação
   * O que é delegado aos módulos

### ✅ Critérios de Saída

* Você pode explicar a ANIMA em 2 minutos **sem mencionar personalidade**
* Você pode diagramar Motor / Seed / Instância em um quadro branco

---

## Fase 1 — Esqueleto do Motor Central (Livre de Identidade)

### 🎯 Objetivo

Criar um SO de raciocínio agnóstico à personalidade.

### 🧠 Construir

* Loop de raciocínio central
* Pipeline Intenção → Plano → Ação
* Registro de capacidades (vazio no início)
* Interface de adaptador (abstração de entrada/saída)
* Abstração de tarefa (mas ainda sem persistência)

### 🚫 Evitar Explicitamente

* Opiniões
* Tom
* Personalidade
* Linguagem "Eu sinto"

### ✅ Critérios de Saída

* Motor pode receber entrada e escolher ações
* Sem comportamento codificado além das regras de segurança
* Motor funciona identicamente independentemente do contexto

---

## Fase 2 — Sistema de Seed

### 🎯 Objetivo

Tornar a identidade uma *preocupação de inicialização*, não uma mutação em tempo de execução.

### 🧬 Construir

1. **Esquema de Seed (v1.0)**

   * Parâmetros de personalidade
   * Restrições comportamentais
   * Política de capacidades
   * Enquadramento narrativo inicial

2. **Validação de Seed**

   * Validação de esquema
   * Verificação de assinatura
   * Compatibilidade de versão

3. **Contrato Motor ↔ Seed**

   * Motor lê seed
   * Motor nunca muta seed
   * Motor aplica restrições definidas pela seed

### 🔐 Segurança

* Seeds são somente leitura após inicialização
* Seeds adulteradas falham completamente

### ✅ Critérios de Saída

* Motor executa com diferentes seeds **sem mudanças de código**
* Mesma entrada + mesma memória + seed diferente → comportamento diferente
* Seed nunca é consultada como "memória"

---

## Fase 3 — Arquitetura de Instância e Memória

### 🎯 Objetivo

Permitir que a ANIMA *cresça* sem deriva de identidade.

### 🧠 Construir

1. **Ciclo de Vida da Instância**

   * Criar instância a partir de motor + seed
   * Inicializar memória vazia
   * Vincular adaptadores

2. **Camadas de Memória**

   * memória de trabalho (efêmera)
   * memória episódica (curto prazo)
   * memória semântica (fatos de longo prazo)
   * memória narrativa (continuidade de identidade)

3. **Regras de Escrita de Memória**

   * O que pode ser armazenado
   * Quem pode acionar escritas
   * Confirmação para memória sensível

### 💡 Importante

* Memória pertence à *instância*, não à seed
* Sem leituras entre instâncias. Nunca.

### ✅ Critérios de Saída

* Reiniciar uma instância preserva continuidade de identidade
* Duas instâncias com mesma seed se sentem diferentes após a memória de trabalho divergir

---

## Fase 4 — Sistema de Capacidades e Controle

### 🎯 Objetivo

Tornar o poder explícito, auditável e controlável.

### 🧩 Construir

1. **Interface de Capacidade**

   Exemplos práticos:
   * Nome
   * Nível de risco
   * Permissões necessárias
   * Requisitos de licença

2. **Pipeline de Execução**
   * Busca de capacidade
   * Verificações de permissão
   * Sandboxing de execução
   * Registro e auditoria

3. **Classificação de Perigo**

   Exemplos:
   * Seguro
   * Sensível
   * Perigoso

### 🔒 Exemplos

* Controle de robô = perigoso
* Acesso a arquivos = sensível
* Chat = seguro

### ✅ Critérios de Saída

* Motor não pode executar ações sem passar pelo portão
* Capacidades podem ser adicionadas/removidas sem tocar na lógica central

---

## Fase 5 — Sistema de Tarefas (Consciência de Longa Duração)

### 🎯 Objetivo

Permitir atividades persistentes e inspecionáveis.

### 🕰️ Construir

* Tarefas persistentes
* Pausa/retomada de tarefas
* Propriedade e permissões de tarefas
* Desligamento seguro e recuperação

### 🧠 Exemplos

* Loop de streaming
* Monitoramento de chat
* Tarefa de pesquisa de longo prazo

### ✅ Critérios de Saída

* Tarefas sobrevivem a reinicializações
* Tarefas respeitam controle de capacidades
* Tarefas podem ser inspecionadas e canceladas

---

## Fase 6 — Ecossistema de Adaptadores

### 🎯 Objetivo

Adaptadores abstraem entrada/saída sem vazar lógica.

### 🔌 Construir

* Adaptador de texto
* Adaptador de voz
* Adaptador Discord
* (Depois) Adaptador de streaming (OBS / VTuber)
* (Depois) Adaptador de robô

### 🔑 Regras

* Adaptadores nunca contêm lógica
* Adaptadores nunca ignoram permissões
* Adaptadores são substituíveis

### ✅ Critérios de Saída

* Mesma instância funciona em múltiplos adaptadores
* Nenhum comportamento específico de adaptador vaza para o motor

---

## Fase 7 — Streaming / Instância Prime

### 🎯 Objetivo

Criar uma encarnação especial de ANIMA para streaming.

### 🌟 Construir

* Seed Prime (assinada, restrita)
* Adaptador de streaming
* Conjunto de capacidades seguro para público
* Políticas de moderação fortes

### 🚫 Regra Explícita

Sem código de caso especial.
Se streaming precisar, *todos* recebem a abstração.

### ✅ Critérios de Saída

* ANIMA de streaming usa o mesmo motor
* Seed Prime não pode ser usada fora do contexto autenticado

---

## Fase 8 — Licenciamento e Produtização

### 🎯 Objetivo

Tornar a ANIMA sustentável.

### 💳 Construir

* Serviço de verificação de licença
* Períodos de graça offline
* Mapeamento de nível de capacidade
* Suporte a mercado de seeds

### 🧠 Vender

* Acesso ao motor
* Desbloqueios de capacidade
* Seeds curadas
* Atualizações e suporte

### ✅ Critérios de Saída

* Motor não licenciado ainda funciona (limitado)
* Licenciamento apenas controla *poder*, não identidade

---

## Fase 9 — Controle de Custos e Otimização

### 🎯 Objetivo

Manter a ANIMA acessível para executar.

### 💸 Construir

* Orçamento de tokens
* Resumo de memória + embeddings (com fallback bruto)
* Limitação de tarefas
* Suspensão / ativação de instância

### ✅ Critérios de Saída

* Custo mensal previsível
* Sem crescimento descontrolado de memória
* Transparência de custos visível ao usuário

---

## Fase 10 — Refinamento e Evolução

### 🎯 Objetivo

Deixar a ANIMA crescer com segurança.

### 🌱 Construir

* Atualizações de versão de seed
* Ferramentas de reflexão de memória
* Relatórios de introspecção
* Caminhos de evolução controlados

### ✅ Critérios de Saída

* Usuários entendem *por que* a ANIMA se comporta como ela faz
* Mudanças parecem orgânicas, não aleatórias
