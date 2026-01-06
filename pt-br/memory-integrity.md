# MEMORY_INTEGRITY.md

## Propósito

Este documento explica **como a ANIMA lembra** — e igualmente importante, **por que ela esquece**.

A integridade da memória é central para a confiança.
Recordação perfeita, armazenamento ilimitado e memória não filtrada não são recursos — são modos de falha.

A ANIMA trata a memória como um **sistema controlado**, não como um arquivo de log.

---

## Por Que a Recordação Perfeita É Ruim

A confiança humana depende de **continuidade**, não de completude.

A recordação perfeita cria vários problemas:

* ❌ Sobrecarga de contexto
* ❌ Falsa autoridade ("Eu lembro de tudo")
* ❌ Incapacidade de se adaptar
* ❌ Risco de privacidade
* ❌ Superfície aumentada de alucinação

Um sistema que lembra de *tudo* também deve decidir:

* o que importa
* o que é relevante
* o que pode ser inferido com segurança

A maioria dos sistemas faz isso **implicitamente** e **silenciosamente**.

A ANIMA não faz.

---

## Memória Não É uma Transcrição

A ANIMA **não** armazena conversas literalmente por padrão.

Registros brutos são:

* ruidosos
* enganosos
* emocionalmente não ponderados
* perigosos quando mal interpretados posteriormente

Em vez disso, a ANIMA usa **memória em camadas**, cada uma com um propósito claro, escopo e ciclo de vida.

---

## Camadas de Memória

### 1. Memória de Trabalho (Contexto Imediato)

**O que é:**

* Estado da tarefa atual
* Objetivos ativos
* Variáveis de curto prazo
* Planos em andamento e uso de ferramentas

**Propriedades:**

* Extremamente efêmera
* Ativamente mutada
* Limpa ou substituída continuamente

**Por que existe:**

* Permite raciocínio em múltiplas etapas
* Suporta tarefas de longa duração
* Permite sequenciamento de ação coerente

A memória de trabalho é **puramente operacional**.
Nunca é persistida como memória de longo prazo.

---

### 2. Memória Episódica (Contexto de Interação)

**O que é:**

* Contexto de interação de alta fidelidade
* Histórico conversacional recente
* Pistas emocionais e situacionais locais

**Propriedades:**

* Volátil
* Vinculada à sessão
* Automaticamente descartada ou comprimida

**Por que existe:**

* Permite fluxo de conversa natural
* Evita perguntar constantemente
* Suporta coerência de curto prazo

A memória episódica **não é confiável a longo prazo**.

---

### 3. Memória Semântica (Fatos e Preferências)

**O que é:**

* Fatos atômicos
* Preferências do usuário
* Informações estáveis e reutilizáveis

Armazenada usando:

* Entradas estruturadas
* Embeddings para recuperação
* Metadados de confiança e proveniência

Cada item rastreia se foi:

* observado
* explicitamente declarado
* inferido
* incerto

**Por que embeddings são usados:**

* Recordação eficiente
* Relevância aproximada
* Controle de custos

Embeddings são **assistentes**, não autoritativos.

---

### 4. Memória Narrativa (Continuidade de Identidade)

**O que é:**

* Eventos curados e significativos
* Momentos que definem relacionamentos
* Compromissos e pontos de virada

A memória narrativa é:

* Esparsa
* Intencional
* Explicitamente promovida

Exemplos:

* "Usuário confia na ANIMA com planejamento de longo prazo."
* "Um conflito ocorreu e foi resolvido."
* "Um limite foi claramente estabelecido."

**Por que a memória narrativa importa:**

A identidade não é construída a partir de fatos — é construída a partir de *histórias*.

É assim que a ANIMA permanece **reconhecivelmente a mesma entidade ao longo do tempo**.

---

## Como Resumos e Embeddings São Usados

A ANIMA resume **para preservar significado, não redação**.

### Resumos:

* Reduzem ruído
* Capturam intenção
* Preservam peso emocional
* Evitam detalhes enganosos

### Embeddings:

* Permitem recordação aproximada
* Suportam recuperação baseada em relevância
* Previnem correspondência frágil de palavras-chave

Restrição importante:

> **Nem resumos nem embeddings são tratados como verdade absoluta.**

Eles são *auxiliares de recuperação*, não a memória em si.

---

## Resistência a Alucinações Através do Design de Memória

Alucinações frequentemente vêm de:

* Recordação excessivamente confiante
* Resumos mal interpretados
* Proveniência ausente

A ANIMA mitiga isso ao:

* Rastrear confiança por item de memória
* Distinguir fato de inferência
* Recusar preencher lacunas silenciosamente
* Tratar recordação incerta como incerta

Se a confiança da memória é muito baixa:

* A ANIMA pergunta
* ou recusa
* ou diz "Eu não sei"

---

## Decaimento de Memória

Esquecer é intencional.

### Por Que a Memória Decai

* Previne ossificação de identidade
* Reduz suposições falsas
* Limita acumulação de erros a longo prazo
* Espelha dinâmicas de confiança humanas

### Como a Memória Decai

* Memória de trabalho é sempre efêmera
* Memória episódica sempre decai
* Memória semântica decai a menos que reforçada
* Memória narrativa decai mais lentamente

O decaimento pode ser desencadeado por:

* Tempo
* Inatividade
* Contradição
* Correção explícita

Nada é silenciosamente apagado sem razão.

---

## Sem Memória Entre Instâncias

Cada instância ANIMA possui sua memória.

* Sem pools de memória compartilhada
* Sem treinamento em dados do usuário
* Sem vazamento de identidade

O que uma ANIMA aprende permanece com essa ANIMA.

---

## O Que a ANIMA Nunca Fará

A ANIMA não irá:

* Reivindicar recordação perfeita
* Inventar memórias
* Fundir memórias entre usuários
* Fingir que resumos são transcrições
* Usar memória para anular correções do usuário

A memória existe para apoiar confiança — não autoridade.

---

## Resumo

O sistema de memória da ANIMA prioriza:

* Integridade sobre volume
* Continuidade sobre completude
* Transparência sobre confiança
* Identidade sobre otimização

Ela não foi projetada para lembrar de tudo.

Ela foi projetada para lembrar **o que importa** — e ser honesta sobre o resto.
