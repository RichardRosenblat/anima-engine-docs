# Guia de Manutenção de Documentação

**Objetivo:** Este guia explica como a documentação ANIMA é mantida, revisada e atualizada.

---

## Princípios Orientadores

A documentação ANIMA segue estes princípios fundamentais:

1. **Clareza Primeiro** - Precisão e compreensibilidade acima de brevidade
2. **Terminologia Canônica** - Definições no [Canonical Glossary](pt-br/foundations/canonical-glossary.md) nunca devem divergir
3. **Sem Duplicação de Autoridade** - Cada conceito tem uma fonte autoritativa
4. **Honestidade Arquitetural** - Documentar decisões como são, não como desejamos que fossem
5. **Referências Cruzadas Deliberadas** - Linkar para fontes canônicas em vez de repetir conteúdo

---

## Cadência de Revisão de Documentação

### Cronograma Recomendado

* **Revisões Trimestrais** - Verificar:
  * Referências cruzadas quebradas
  * Descrições arquiteturais desatualizadas
  * Divergência de terminologia
  * Inconsistências com implementação (uma vez que o engine seja público)

* **Antes de Lançamentos Principais** - Verificar:
  * Todos os ADRs estão atuais
  * Documentação de arquitetura corresponde à implementação
  * Modelo de segurança reflete restrições reais
  * Exemplos e especificações são precisos

* **Após Mudanças Arquiteturais Significativas** - Atualizar:
  * ADRs afetados (ou criar novos)
  * Documentação de arquitetura
  * Canonical Glossary (se a terminologia mudar)
  * Referências cruzadas em documentos relacionados

### Checklist de Revisão

Ao revisar documentação:

- [ ] Todas as referências cruzadas estão funcionando?
- [ ] A terminologia é consistente com o Canonical Glossary?
- [ ] Os ADRs ainda são precisos ou precisam de atualizações de status?
- [ ] Os exemplos refletem a arquitetura atual?
- [ ] Os non-goals ainda são precisos?
- [ ] O tom é consistente (preciso, calmo, profissional, opinativo quando apropriado)?
- [ ] Há explicações duplicadas que devem ser consolidadas?
- [ ] Os diagramas (baseados em texto ou visuais) ainda são precisos?

---

## Quando Criar um Novo ADR vs. Atualizar um Existente

### Criar um Novo ADR Quando:

* **Uma nova decisão arquitetural é tomada** que afeta estrutura, comportamento ou limites do sistema
* **Uma restrição significativa é adicionada ou removida** (ex: novo limite de segurança, mudança no modelo de capability)
* **Um componente principal é introduzido** (ex: nova camada de memória, novo tipo de módulo)
* **Um ADR existente é substituído** (marque o antigo como "Superseded by ADR-XXX")

### Atualizar um ADR Existente Quando:

* **Clarificando decisões existentes** sem mudar a decisão em si
* **Adicionando contexto ou justificativa** que era implícita mas não escrita
* **Corrigindo erros ou typos** na descrição
* **Adicionando referências cruzadas** para documentos relacionados

### NÃO Atualizar ADRs Para:

* Detalhes de implementação (estes vão em comentários de código ou documentação de implementação)
* Escolhas táticas que não afetam arquitetura
* Soluções temporárias
* Configurações específicas de plataforma

---

## Ciclo de Vida de ADR e Valores de Status

ADRs usam estes valores de status:

* **Proposed** - Em discussão, ainda não adotado
* **Accepted** - Adotado e guiando implementação
* **Deprecated** - Não mais recomendado, mas ainda não substituído
* **Superseded** - Substituído por um ADR mais novo (incluir referência: "Superseded by ADR-XXX")
* **Rejected** - Considerado mas explicitamente rejeitado (manter para contexto histórico)

**Atualizações de status** são apropriadas quando:
* Uma decisão arquitetural não é mais válida
* Uma abordagem melhor substitui uma mais antiga
* Implementação revela que a decisão estava incorreta

**Inclua uma nota** explicando por que o status mudou e linkando para o ADR que o substitui, se aplicável.

---

## Como Propor Mudanças na Documentação

### Para Mudanças Menores (Typos, Formatação, Links Quebrados)

1. **Identifique o problema** (link quebrado, typo, inconsistência de formatação)
2. **Verifique se não é intencional** (cheque documentos relacionados para contexto)
3. **Submeta um pull request** com:
   * Descrição clara do que estava errado
   * O que foi mudado
   * Por que a mudança está correta

### Para Clarificações e Adições

1. **Verifique duplicação de autoridade** - Este conteúdo já está coberto em outro lugar?
2. **Identifique a fonte canônica** - Isto deve ser adicionado a um documento existente ou ser novo?
3. **Siga o tom existente** - Preciso, calmo, profissional, opinativo quando apropriado
4. **Use terminologia canônica** - Referencie o [Canonical Glossary](pt-br/foundations/canonical-glossary.md)
5. **Adicione referências cruzadas** - Linke para fontes autoritativas relacionadas
6. **Submeta um pull request** explicando:
   * Que lacuna a mudança preenche
   * Por que este é o lugar certo para o conteúdo
   * Como mantém consistência com documentação existente

### Para Mudanças Arquiteturais ou Conceituais

1. **Discuta primeiro** - Mudanças arquiteturais requerem consenso
2. **Determine se um novo ADR é necessário** (veja "Quando Criar um Novo ADR" acima)
3. **Esboce a mudança** mantendo:
   * Consistência com terminologia canônica
   * Alinhamento com decisões arquiteturais existentes
   * Clareza e precisão
4. **Atualize referências cruzadas** - Encontre todos os documentos que referenciam o conceito alterado
5. **Submeta um pull request** com:
   * Justificativa para a mudança
   * Impacto na documentação existente
   * Lista de referências cruzadas afetadas

---

## Atualizações do Canonical Glossary

O [Canonical Glossary](pt-br/foundations/canonical-glossary.md) é **autoritativo**. Mudanças requerem cuidado especial.

### Quando Atualizar o Glossary

* Um novo componente arquitetural é introduzido
* Uma definição existente é ambígua ou incompleta
* Divergência de terminologia é detectada
* Um ADR introduz novos termos canônicos

### Como Atualizar o Glossary

1. **Verifique se o termo é arquitetural** - Detalhes táticos ou de implementação não pertencem aqui
2. **Verifique conflitos** - Isto conflita com definições existentes?
3. **Escreva a definição com precisão** - Seja completo mas conciso
4. **Adicione referências cruzadas** - Linke para ADRs e documentos de arquitetura relacionados
5. **Atualize todo uso** - Busque o termo na documentação e garanta uso consistente
6. **Obtenha revisão** - Mudanças no glossary afetam todo o conjunto de documentação

**Nunca** mude uma definição canônica existente sem:
* Documentar a razão da mudança
* Atualizar todas as referências ao termo
* Considerar se uma atualização de status de ADR é necessária

---

## Estrutura e Organização da Documentação

### Estrutura de Diretório Atual

```
/
├── README.md                  # Ponto de entrada e visão geral de alto nível
├── GETTING_STARTED.md         # Guia de navegação para novos leitores
├── FAQ.md                     # Perguntas comuns com respostas concisas
├── DOCUMENTATION_MAINTENANCE.md # Este documento
├── adr/                       # Architecture Decision Records (numerados)
├── architecture/              # Documentação detalhada de arquitetura
├── foundations/               # Princípios canônicos e glossário
├── vision/                    # Visão, objetivos e non-goals
├── governance/                # Licenciamento e viabilidade do projeto
├── safety/                    # Modelo de segurança, modelo de ameaças, integridade de memória
├── specs/                     # Especificações técnicas (URN, JSRS)
├── roadmaps/                  # Roadmap de desenvolvimento
└── announcements/             # Anúncios do projeto
```

### Adicionando Novos Documentos

**Antes de adicionar um novo documento:**

1. **Verifique se o conteúdo pertence a um documento existente** - Evite fragmentação
2. **Determine o diretório apropriado** - Siga a estrutura existente
3. **Garanta que tem um propósito claro e único** - Sem duplicação de autoridade
4. **Use um nome de arquivo descritivo** - Minúsculas, separado por hífen (ex: `new-concept.md`)

**Ao adicionar um novo documento:**

1. Inclua uma **declaração de objetivo** no topo
2. Adicione **referências cruzadas** para documentos relacionados
3. Atualize documentos de navegação (README, GETTING_STARTED.md, READMEs de diretório relevantes)
4. Use terminologia canônica
5. Siga convenções de formatação existentes

### Removendo ou Renomeando Documentos

**Evite renomear ou remover documentos.** Isso quebra links externos e atrapalha navegação.

**Se um documento deve ser renomeado:**
* Crie um redirecionamento ou stub no local antigo apontando para o novo
* Atualize todas as referências cruzadas
* Documente a mudança em announcements

**Se um documento deve ser removido:**
* Garanta que o conteúdo seja preservado em outro lugar (se ainda válido)
* Atualize todas as referências cruzadas
* Considere deixar um stub explicando para onde o conteúdo foi movido

---

## Manutenção de Referências Cruzadas

Referências cruzadas são críticas para navegação. Links quebrados comprometem a qualidade da documentação.

### Melhores Práticas

* **Use caminhos relativos** - `[Canonical Glossary](pt-br/foundations/canonical-glossary.md)`
* **Linke para fontes autoritativas** - Não repita conteúdo, linke para ele
* **Use texto de link descritivo** - Não "clique aqui", mas "veja [Seed System](pt-br/architecture/seed-system.md)"
* **Verifique links antes de commitar** - Especialmente após movimentações ou renomeações de arquivos

### Encontrando Links Quebrados

Periodicamente verifique por links quebrados:

```bash
# Encontre todos os links markdown
grep -r "\[.*\](.*\.md)" *.md */*.md

# Verifique se arquivos referenciados existem
# (Verificação manual ou verificador de links automatizado)
```

---

## Diretrizes de Tom e Estilo

A documentação ANIMA usa um tom **preciso, calmo, profissional e opinativo**.

### Faça:
* Seja preciso e inequívoco
* Declare decisões clara e confiantemente
* Explique justificativa sem ser defensivo
* Use "ela" para ANIMA (quando se referir a uma instância, não ao engine)
* Use terminologia técnica corretamente
* Seja honesto sobre limitações e restrições

### Não Faça:
* Use linguagem de marketing ou hype
* Hesite desnecessariamente ("talvez", "quem sabe", "pode")
* Peça desculpas por decisões de design
* Use humor à custa de clareza
* Suavize limites de segurança ou restrições
* Introduza nova terminologia sem definições canônicas

### Exemplos

**Bom:**
> "ANIMA não é projetado para operar como um agente autônomo de internet. Este é um limite explícito escolhido para proteger segurança, não uma limitação temporária."

**Ruim:**
> "Ainda não estamos prontos para acesso à internet, mas podemos adicionar isso depois se conseguirmos torná-lo seguro o suficiente."

**Bom:**
> "O Cognitive Kernel supervisiona execução através de módulos controlados por lease. Esta invariante arquitetural garante que nenhuma ação ocorra sem autorização criptográfica válida."

**Ruim:**
> "Usamos um sistema de lease legal que meio que garante que as coisas sejam autorizadas."

---

## Changelog de Documentação (Opcional)

Para atualizações principais de documentação, considere manter um changelog leve:

### Formato

```markdown
## [Data] - [Tipo de Mudança]

### Adicionado
- Novo documento: [Título](caminho/para/documento.md)
- Nova seção em [Documento](caminho.md): [Nome da Seção]

### Alterado
- Atualizado [Documento](caminho.md): [Descrição da mudança]
- Revisado status de ADR-XXX para [Novo Status]

### Corrigido
- Corrigidos links quebrados em [Documento](caminho.md)
- Corrigida inconsistência de terminologia em [Documento](caminho.md)
```

### Quando Atualizar

* Reestruturação principal de documentação
* Novos ADRs publicados
* Adições significativas de conteúdo
* Mudanças de terminologia no Canonical Glossary

---

## Resumo do Processo

### Para Contribuidores de Documentação

1. **Leia a documentação existente primeiro** - Entenda o estado atual
2. **Siga o guia de estilo** - Mantenha consistência
3. **Use terminologia canônica** - Verifique o Glossary
4. **Adicione referências cruzadas** - Linke para fontes autoritativas
5. **Teste todos os links** - Garanta que funcionam
6. **Seja preciso** - Clareza sobre criatividade
7. **Obtenha revisão** - Mudanças na documentação afetam todos os leitores

### Para Revisores de Documentação

1. **Verifique consistência de terminologia** - Corresponde ao Glossary?
2. **Verifique referências cruzadas** - Os links estão corretos e apropriados?
3. **Avalie o tom** - É preciso, calmo, profissional, opinativo?
4. **Procure duplicação** - A autoridade está dividida entre múltiplos documentos?
5. **Avalie a estrutura** - Este é o lugar certo para este conteúdo?
6. **Considere o impacto** - Como esta mudança afeta documentos relacionados?

---

## Contato e Discussão

Para perguntas sobre documentação:
* Revise issues e pull requests existentes
* Proponha mudanças via pull requests
* Inclua justificativa e contexto nas descrições de PR

---

**Última Atualização:** 28 de janeiro de 2026
