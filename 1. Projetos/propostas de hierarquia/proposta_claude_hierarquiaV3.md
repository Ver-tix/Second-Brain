---
tipo:
  - sintese
dominio:
  - IA
subdominio:
  - RAG
---
# Guia Mestre — Metadados do Second Brain

> Documento de referência para uso por agentes de IA (Claude) como contexto de consulta.
> Consolida a estrutura hierárquica, os tipos de nota, os campos de metadata, as decisões de design, o sistema de agentes de IA e o roteiro de implementação do vault.
> Este documento substitui/consolida as versões anteriores — nenhuma decisão antiga foi removida, apenas atualizada ou complementada (ver seção 6 para o histórico de mudanças).

---

## 1. Propósito

Este vault funciona como Second Brain e também como material de consulta (RAG) para agentes de IA. Por isso, a estrutura de metadata prioriza **legibilidade direta do arquivo `.md` bruto**, não apenas a renderização visual do Obsidian. Qualquer plugin usado (ex: Breadcrumbs) é tratado como "lente" — o dado relacional real vive no frontmatter YAML, e deve sobreviver independente do plugin estar instalado.

---

## 2. Estrutura geral do vault

```
Second Brain
├── Áreas (domínios amplos e permanentes)
│   └── Projetos (iniciativas com início/fim)
│       └── Subprojetos / Componentes
│           └── Notas atômicas
```

---

## 3. Hierarquia de departamentos (Business como raiz) — princípio fractal

**Atualização de estrutura:** Business é tratado como departamento-raiz administrativo ("CEO"), com Marketing, Branding, Inteligência Artificial, Vendas e Finanças subordinados abaixo dele. **Isso substitui a definição anterior, em que os domínios eram tratados como irmãos sem hierarquia** (ver histórico em 6.5).

**Princípio fractal (novo — ver 6.9):** a régua Estratégico > Tático > Operacional não é única e absoluta pro vault inteiro. Ela se repete recursivamente dentro de cada departamento, e o `nivel` de qualquer nota é **relativo ao seu pai imediato** (`up`), não ao topo da árvore. Exemplos:
- **Produto**, visto do CEO, é operacional (dois níveis abaixo); visto do CMO, é uma das 4 categorias táticas do mix; dentro do próprio microverso de Produto, reaparece a régua inteira (estratégico: product-market fit; tático: priorização de roadmap; operacional: specs de feature).
- **IA (CTO)**, vista do CEO, é um departamento tático (mesmo nível que Marketing/Branding); dentro do microverso do CTO, reaparece a régua inteira (estratégico: arquitetura de agentes; tático: RAG, orquestração; operacional: prompt específico).
- O mesmo vale pra Preço, Praça e Promoção — cada um tem seu próprio microverso interno.

```
business (CEO)
  proposta-de-valor
  segmentos-de-cliente
  canais
  relacionamento-com-cliente
  fontes-receita
  estrutura-custos
  recursos-chave
  atividades-chave
  parcerias-chave
  inovacao

  marketing-estrategico (CMO)
    marketing-tatico-mix
      marketing-tatico-preco
      marketing-tatico-praca
      marketing-tatico-promocao
      marketing-tatico-produto   # microverso fractal — pode ter seu próprio agente (cpo), subordinado ao cmo

  brand-strategy (CBO)
    branding-tatico
      tatico-identidade_verbal-naming
      tatico-identidade_verbal-tom_de_voz
      tatico-identidade_verbal-messaging
      tatico-identidade_verbal-ativos_distintivos_verbais
      tatico-identidade_visual-logo
      tatico-identidade_visual-tipografia
      tatico-identidade_visual-cores
      tatico-identidade_visual-ativos_distintivos_visuais
      tatico-personalidade-arquetipos
      tatico-experiencia_de_marca-cultural_branding
      tatico-experiencia_de_marca-touchpoints
      tatico-experiencia_de_marca-atendimento
      tatico-experiencia_de_marca-embalagem

  inteligenciaartificial (CTO)

  vendas (CSO)

  financas (CFO)
```

**Nota sobre os rótulos (CMO)/(CBO)/(CTO)/(CSO)/(CFO):** são só títulos de cargo na árvore, não renomeiam o slug — `dominio` continua `marketing`, `branding`, `inteligenciaartificial`, `vendas`, `financas` em toda nota, normalmente.

**Nota sobre o bloco Business:** os 9 blocos do BMC aparecem completos de propósito. Alguns (ex: Proposta de Valor ↔ Produto de Marketing; Canais ↔ Praça de Marketing) foram granulados de forma mais fina dentro de Marketing/Branding — a sobreposição entre esses blocos e as categorias táticas dos outros departamentos ainda não foi resolvida, mas o princípio fractal (3, 6.9) abre um caminho pra reformular essa pendência como "mesma questão vista de duas altitudes diferentes" em vez de redundância (ver 6.8). "Inovação" é uma adição nova, fora dos 9 blocos originais do BMC.

**Antes de usar os slugs no frontmatter**, escreva por extenso a árvore completa dos seus departamentos (igual ao exemplo acima), pra fixar os nomes exatos e evitar variações tipo `branding-tatico-preço` numa nota e `branding-tatico-preco` (sem acento) em outra. **Convenção adotada: slugs de campos controlados (`nivel`, `dominio`, `subdominio`, `sub_subdominio`, `setor`, `agente`) sempre sem acento** — acentos ficam liberados só em `tags` de classificação livre e em texto corrido (ver 6.10).

Essa hierarquia administrativa é documentada **uma vez só**, no MOC raiz (`Business HUB.md`), via campo `departamentos`. Não existe campo `dominio_raiz` ou `fluxo` repetido em cada nota individual — um campo que sempre vale "business" em toda nota não discrimina nada no retrieval e só custa tokens à toa (ver 6.7).

Dentro de cada departamento, a relação `up/down/same` do Breadcrumbs continua descrevendo a estrutura **interna** do próprio domínio (ex: Business → seus 9 blocos do BMC; Marketing → seus 4 Ps; Branding → suas 4 categorias de identidade) — não a relação entre departamentos em si, que fica centralizada no campo `departamentos`. Se quiser que o Breadcrumbs também renderize visualmente o vínculo Marketing/Branding → Business, basta adicionar `up: "[[Business]]"` nas notas-mãe de cada departamento — isso é opcional e complementar.

### 3.1 Categorias táticas fixas por domínio

| Domínio | Categorias táticas (fixas) |
|---|---|
| **Marketing** | Preço, Praça, Produto, Promoção |
| **Business** | Proposta de Valor, Segmentos de Cliente, Canais, Relacionamento com Cliente, Fontes de Receita, Estrutura de Custos, Recursos-Chave, Atividades-Chave, Parcerias-Chave (9 blocos do Business Model Canvas) |
| **Branding** | Identidade Verbal, Identidade Visual, Personalidade de Marca, Experiência de Marca |
| **Inteligência Artificial** | Ainda não definidas |
| **Vendas** | Ainda não definidas |
| **Finanças** | Ainda não definidas |

---

## 4. Os 4 tipos de nota

Toda nota do vault se encaixa em um destes 4 tipos, identificado pelo campo `tipo`. Arquivos "HUB" (Business HUB, Marketing HUB, Branding HUB, IA HUB) são do tipo `moc` — só precisam ganhar os campos abaixo, sem mudar de nome nem de conteúdo.

**Nota de fusão (ver 6.10):** uma proposta externa (opencode) sugeriu substituir esse eixo por `tipo: livro/capítulo/método/exemplo/case/guia` — decisão explícita de **manter este eixo como está**. Os dois eixos descrevem coisas diferentes (este é epistêmico — qual é a função da nota pro RAG; o outro é estrutural — qual é a forma do documento). Se a granularidade estrutural for útil no futuro, ela pode entrar como campo adicional, não como substituto deste.

### 4.1 `moc`

Nota-índice que organiza e aponta pra outras notas — não tem conteúdo autoral próprio, só links e metadados estruturais.

```yaml
tipo: moc
dominio: branding
autoridade_padrao:
  - "Byron Sharp & Jenni Romaniuk"
  - "Douglas Holt"
  - "Aaker & Kotler"
  - "Marty Neumeier"
```

**Campo `autoridade_padrao`:** só existe nos MOCs de domínio (não em `fonte`/`conceito`/`sintese`). Define a ordem de desempate quando há divergência entre autores e o usuário não especificou de quem quer a visão.

### 4.2 `fonte`

Uma nota sobre um livro, capítulo ou autor específico.

```yaml
tipo: fonte
autor: "Byron Sharp"
obra: "How Brands Grow"
dominio: branding
subdominio: branding-estrategico
sub_subdominio: ""
tier: 1
status: consenso
```

### 4.3 `conceito`

Uma ideia que atravessa várias fontes/autores diferentes, com ângulos distintos.

```yaml
tipo: conceito
dominio: branding
subdominio: branding-estrategico
autores_relacionados: ["Byron Sharp", "Jenni Romaniuk", "Douglas Holt"]
tier: 1
status: contestado
```

### 4.4 `sintese`

Posição própria, já resolvida sobre um conflito entre fontes. É Tier 0 — a nota mais valiosa pro RAG.

```yaml
tipo: sintese
dominio: branding
subdominio: branding-estrategico
tier: 0
status: sua_posicao_atual
resolve_conflito_entre: ["Byron Sharp", "Douglas Holt"]
```

### 4.5 Tabela-resumo de quando usar cada tipo

|Situação da nota|Tipo|
|---|---|
|É um HUB que organiza e aponta pra outras notas|`moc`|
|É sobre um livro/autor/capítulo específico|`fonte`|
|É uma ideia que aparece em várias fontes diferentes|`conceito`|
|É a posição própria sobre algo já debatido|`sintese`|

---

## 5. Campos de metadata (frontmatter) — tabela consolidada

| Campo | Tipo | Função | Onde aparece |
|---|---|---|---|
| `tags` | lista | Tag aninhada nativa do Obsidian, ex: `dominio/business`, `nivel/tatico` | toda nota |
| `nivel` | texto | Um de: `estrategico`, `tatico`, `operacional` — **relativo ao pai imediato** (`up`), não absoluto ao vault (ver 6.9) | toda nota |
| `dominio` | texto | Departamento: `business`, `marketing`, `branding`, `inteligenciaartificial`, `vendas`, `financas` | toda nota |
| `subdominio` | texto (slug) | Nível tático dentro do departamento, formato `dominio-nivel` (ex: `branding-estrategico`) — granularidade extra, **complementar** ao `nivel`, não o substitui | `fonte`, `conceito`, `sintese` |
| `sub_subdominio` | texto (slug) | Nível operacional, mais específico (ex: `branding-operacional-promocao-copywriting`) — opcional, deixe vazio se não usar | `fonte`, `conceito`, `sintese` (opcional) |
| `up` | link | Nota-pai (Breadcrumbs) | toda nota |
| `down` | lista de links | Notas-filhas (Breadcrumbs) | toda nota |
| `same` | lista de links | Notas laterais / relacionadas, mesmo nível (Breadcrumbs) | toda nota |
| `tipo` | texto | `moc` / `fonte` / `conceito` / `sintese` | toda nota |
| `autor` | texto | Autor da fonte | só `fonte` |
| `obra` | texto | Livro/capítulo de origem | só `fonte` |
| `tier` | número | `0` = síntese própria · `1` = referência curada · `2` = rascunho/nota bruta | `fonte`, `conceito`, `sintese` |
| `status` | texto | `consenso` / `contestado` / `sua_posicao_atual` | `fonte`, `conceito`, `sintese` |
| `autores_relacionados` | lista | Autores que tocam nesse conceito | só `conceito` |
| `resolve_conflito_entre` | lista | Autores/fontes que a síntese resolve | só `sintese` |
| `autoridade_padrao` | lista ordenada | Ordem de desempate entre autores do domínio | só `moc` de domínio |
| `departamentos` | lista | Departamentos subordinados (só no MOC raiz de Business) | só `moc` raiz (Business HUB) |
| `agente` | texto | **(novo, seção 14)** Agente de IA dono do conteúdo — chave de roteamento pro RAG multiagente. Recursivo: qualquer nó com complexidade suficiente pode ter seu próprio agente, subordinado ao agente do nível acima | opcional — mais útil em `moc`/`fonte` que definem escopo de um domínio/setor |
| `setor` | texto (vocabulário controlado) | **(novo, seção 14)** Especialização operacional dentro de um departamento (ex: `copywriting`, `precificacao`, `seo`) — granularidade de roteamento pro agente operacional | opcional, notas operacionais |

Campo `escola` (pra marcar teoria de origem, ex: Sharp vs. Aaker vs. Kotler) foi **avaliado e descartado** — decisão de manter estrutura departamental fixa em vez de marcação por escola teórica em toda nota (ver 6.3).

**Nota de fusão:** a proposta opencode usava o nome `departamento` pro mesmo conceito que já cobríamos com `dominio`. Decisão: manter `dominio` como nome de campo (evita duplicar o mesmo eixo com dois nomes) — `agente` e `setor` são a camada genuinamente nova que se soma por cima da estrutura já existente (ver 6.10).

---

## 6. Decisões e exceções registradas

### 6.1 Brand Equity não é um galho da árvore
Brand Equity (Aaker/Keller) é um framework de **mensuração de resultado**, não uma alavanca de ação — por isso não vira categoria tática em Branding. Objetivos e KPIs de marca ficam definidos **por projeto específico**, como propriedades soltas na nota do projeto (ex: `objetivo:`, `kpi:`), não como parte fixa da hierarquia departamental.

### 6.2 Possível 5º departamento em Branding (pendente)
Se famílias de marca (ex: marca-mãe + sub-marcas) precisarem de tratamento próprio no futuro, considerar adicionar **Arquitetura de Marca** como quinto departamento tático em Branding. Não implementado ainda.

### 6.3 Metodologia de Byron Sharp
Sharp (mental/physical availability, CEPs, ativos distintivos) diverge teoricamente de Aaker (foco em lealdade/diferenciação) e de Kotler/STP (segmentação de nicho). Ainda assim, não recebeu departamento ou campo próprio:
- **CEPs (Category Entry Points)** → tratados como nota conceitual/diagnóstica dentro de **Marketing (Estratégico)**, no mesmo nível que Blue Ocean Strategy fica em Business (Estratégico).
- **Ativos Distintivos** → entram como notas operacionais comuns dentro de **Identidade Visual / Identidade Verbal** (Branding), sem marcação teórica separada.
- Decisão consciente: não isolar por escola teórica (campo `escola` descartado) — prática de notas de escolas diferentes pode conviver na mesma estrutura departamental sem marcação explícita.

### 6.4 Colisão de nomes entre domínios
Termos como "MVP" podem aparecer tanto em Business (Proposta de Valor) quanto em Marketing (Produto). Ao criar a nota, decidir entre:
- Nota única compartilhada, linkada nos `down` de ambos os domínios, ou
- Notas separadas com sufixo (ex: "MVP (Business)" / "MVP (Marketing)").

### 6.5 Histórico — hierarquia de domínios atualizada
Decisão original: Marketing, Business e Branding eram tratados como **irmãos** entre si (relação `same`), sem hierarquia entre eles. **Essa decisão foi revista e substituída**: Business passou a ser tratado como departamento-raiz administrativo ("CEO"), com Marketing, Branding, Inteligência Artificial, Vendas e Finanças subordinados a ele (ver seção 3). Fica registrado aqui para histórico — a estrutura vigente é a da seção 3.

### 6.6 Campo `nivel` mantido junto com `subdominio`/`sub_subdominio`
Avaliado substituir `nivel` pelos slugs compostos (`subdominio`/`sub_subdominio`), mas decidido **manter os dois**: `nivel` continua como campo simples (`estrategico`/`tatico`/`operacional`) pra filtros rápidos via Dataview; `subdominio`/`sub_subdominio` são campos extras que adicionam granularidade (departamento + nível combinados num slug), úteis pra notas `fonte`/`conceito`/`sintese`. Não há redundância eliminada — são dois níveis de granularidade coexistindo.

### 6.7 Sem campo `dominio_raiz` ou `fluxo`
Um campo que sempre vale "business" em toda nota não discrimina nada no retrieval — só custa tokens à toa. A relação de raiz administrativa (Business → Marketing/Branding/IA/Vendas/Finanças) fica documentada uma única vez, no campo `departamentos` do MOC raiz (`Business HUB.md`).

### 6.8 Pendente — sobreposição entre blocos do BMC e táticas de Marketing/Branding
A lista completa dos 9 blocos do BMC foi restaurada em Business (seção 3), mas alguns blocos foram granulados de forma mais fina dentro de Marketing e Branding (ex: Proposta de Valor ↔ Produto de Marketing; Canais ↔ Praça de Marketing). Ainda não foi decidido como evitar redundância entre esses blocos e as categorias táticas dos outros departamentos. **Atualização:** o princípio fractal (6.9) sugere uma via de solução — talvez não seja redundância, e sim a mesma questão de negócio vista de duas altitudes diferentes (Proposta de Valor = altitude do CEO; Produto = altitude do CMO). Ainda não resolvido, fica como hipótese de trabalho pra discussão futura.

### 6.9 Hierarquia fractal — `nivel` é relativo ao pai imediato, não absoluto
A régua Estratégico > Tático > Operacional se repete recursivamente dentro de cada departamento e de cada subdivisão dele — não é uma régua única aplicada uma vez só ao vault inteiro. O `nivel` de qualquer nota é relativo ao seu pai imediato (`up`), não ao topo da árvore (Business/CEO).

Exemplos:
- **Produto**: operacional na visão do CEO · tático na visão do CMO · dentro do próprio microverso de Produto, reaparece estratégico/tático/operacional (ex: product-market fit / priorização de roadmap / specs de feature).
- **Inteligência Artificial (CTO)**: tático na visão do CEO (mesmo nível que Marketing/Branding) · dentro do próprio microverso do CTO, reaparece estratégico/tático/operacional (ex: arquitetura de agentes / RAG e orquestração / prompt específico).
- O mesmo padrão vale pra Preço, Praça e Promoção, e por extensão a qualquer nó da árvore com complexidade suficiente pra merecer sua própria régua interna.

Essa recursividade é o que sustenta o campo `agente` como recursivo também (ver 6.10, seção 14): qualquer nó fractal pode, em tese, ter seu próprio agente subordinado ao agente do nível acima.

### 6.10 Incorporação da proposta opencode (sistema de agentes de IA / roteamento RAG)
Uma proposta externa (opencode) trouxe um sistema de metadata voltado a roteamento RAG por agente de IA, testado em 515 arquivos de `3. Recursos/Estudos/Livros` e `Métodos`. Decisões de fusão:
- **`tipo` mantido como está** (moc/fonte/conceito/sintese) — não substituído pelo eixo estrutural (livro/capítulo/método/...) da proposta original (ver 4).
- **`departamento` não vira campo novo** — mapeia 1:1 pro `dominio` já existente; evita duplicar o mesmo eixo com dois nomes (ver 5).
- **`agente` e `setor` incorporados como campos novos** (ver 5, seção 14) — essa é a parte genuinamente nova: um dono explícito de roteamento por documento, mais um vocabulário controlado de especialização operacional.
- **Acentuação padronizada sem acento** em todos os campos controlados (`nivel`, `dominio`, `subdominio`, `sub_subdominio`, `setor`, `agente`) — a proposta original tinha inconsistência entre `estratégico`/`tático` (com acento) e o padrão já estabelecido (`estrategico`/`tatico`, sem acento); resolvido a favor do padrão sem acento, já usado em todo o resto do documento.
- **Organograma corrigido pra respeitar o princípio fractal (6.9)**: a proposta original tratava `cpo` (Produto) como departamento irmão de `cmo`/`cbo`, reportando direto ao `ceo`. Isso contradiz a lógica fractal — Produto é tático dentro do microverso de Marketing, não um departamento de mesma altitude. Corrigido: `cpo` (quando existir) reporta ao `cmo`, não ao `ceo` (ver 14.2).
- **Vendas (CSO) e Finanças (CFO)** foram incorporados como departamentos legítimos de primeiro nível (siblings de Marketing/Branding/IA sob Business) — diferente do caso de Produto, esses não são parte do mix de Marketing, são funções de negócio genuinamente separadas.

---

## 7. Templates de frontmatter por tipo de nota

Ver exemplos completos em `moc`/`fonte`/`conceito`/`sintese` na seção 4. Esses campos combinam com os templates por nível hierárquico abaixo (seção 8) — uma nota real normalmente carrega os dois conjuntos de campos juntos (ver exemplo combinado, seção 9).

## 8. Templates de frontmatter por nível hierárquico (Breadcrumbs)

### 8.1 Nota-mãe (Estratégico)
```yaml
---
tags:
  - dominio/<business|marketing|branding|inteligenciaartificial|vendas|financas>
  - nivel/estrategico
nivel: estrategico
dominio: <business|marketing|branding|inteligenciaartificial|vendas|financas>
up: "[[Área - Negócios e Marketing]]"
down:
  - "[[<categoria tática 1>]]"
  - "[[<categoria tática 2>]]"
same:
  - "[[<domínio vizinho 1>]]"
---
```

### 8.2 Nota tática
```yaml
---
tags:
  - dominio/<business|marketing|branding|inteligenciaartificial|vendas|financas>
  - nivel/tatico
nivel: tatico
dominio: <business|marketing|branding|inteligenciaartificial|vendas|financas>
up: "[[<Nota-mãe do domínio>]]"
down:
  - "[[<ferramenta operacional 1>]]"
  - "[[<ferramenta operacional 2>]]"
same:
  - "[[<categoria tática vizinha 1>]]"
  - "[[<categoria tática vizinha 2>]]"
---
```

### 8.3 Nota operacional
```yaml
---
tags:
  - dominio/<business|marketing|branding|inteligenciaartificial|vendas|financas>
  - nivel/operacional
nivel: operacional
dominio: <business|marketing|branding|inteligenciaartificial|vendas|financas>
up: "[[<Nota tática correspondente>]]"
same:
  - "[[<ferramenta operacional vizinha 1>]]"
  - "[[<ferramenta operacional vizinha 2>]]"
---
```

---

## 9. Exemplo combinado completo

Uma nota `fonte` real, unindo os três sistemas de campos (tipo/tier/status + up/down/same + agente/setor):

```yaml
---
tags:
  - dominio/branding
  - nivel/estrategico
tipo: fonte
autor: "Byron Sharp"
obra: "How Brands Grow"
dominio: branding
nivel: estrategico
subdominio: branding-estrategico
sub_subdominio: ""
tier: 1
status: consenso
agente: cbo
up: "[[Branding]]"
same:
  - "[[Douglas Holt - Como as Marcas Se Tornam Ícones]]"
  - "[[Aaker - Brand Equity]]"
---
```

---

## 10. Exemplo aplicado — domínio Business

**Nota-mãe (Business — Estratégico):**
```yaml
---
tags:
  - dominio/business
  - nivel/estrategico
nivel: estrategico
dominio: business
agente: ceo
up: "[[Área - Negócios e Marketing]]"
down:
  - "[[Business - Proposta de Valor]]"
  - "[[Business - Segmentos de Cliente]]"
  - "[[Business - Canais]]"
  - "[[Business - Relacionamento com Cliente]]"
  - "[[Business - Fontes de Receita]]"
  - "[[Business - Estrutura de Custos]]"
  - "[[Business - Recursos-Chave]]"
  - "[[Business - Atividades-Chave]]"
  - "[[Business - Parcerias-Chave]]"
---
```

**Business HUB.md (moc raiz — registra a hierarquia administrativa):**
```yaml
---
tipo: moc
dominio: business
descricao: "Organograma administrativo raiz do Second Brain"
departamentos:
  - marketing
  - branding
  - inteligenciaartificial
  - vendas
  - financas
---
```

**Nota tática (Business — Fontes de Receita):**
```yaml
---
tags:
  - dominio/business
  - nivel/tatico
nivel: tatico
dominio: business
up: "[[Business]]"
down:
  - "[[Freemium]]"
  - "[[Assinatura]]"
  - "[[Comissão]]"
same:
  - "[[Business - Estrutura de Custos]]"
  - "[[Business - Proposta de Valor]]"
---
```

**Nota operacional (Freemium):**
```yaml
---
tags:
  - dominio/business
  - nivel/operacional
nivel: operacional
dominio: business
up: "[[Business - Fontes de Receita]]"
same:
  - "[[Assinatura]]"
  - "[[Comissão]]"
---
```

---

## 11. Roteiro de implementação

### Passo 0 — Backup de segurança
1. Confirme que o vault está com todos os commits em dia no GitHub.
2. Crie uma branch nova pra essa migração: `git checkout -b reforma-metadados`.

### Passo 1 — Limpeza básica de tags
1. Remover o `#` de dentro do campo `tags` (6 arquivos): `#estratégia`, `#Funding`, `#descoberta`, `#dicionario`, `#inteligenciaartificial`, `#tecnologia`.
2. Corrigir o typo: `brandig` → `branding`.
3. Unificar duplicatas singular/plural: `case`/`cases`, `funil`/`funis`, `tático`/`táticas` — escolher uma forma pra cada.
4. Padronizar capitalização: lowercase geral, ajustando `BrandEquity`, `RealEstate`, `Funding`, `Prompts`, `RAG`.
5. Revisar as 6 tags órfãs restantes: `brandDesign`, `comportamental`, `instruction`, `luxo`, `segmentação`, `vendas`.
6. Remover chaves de metadata vazadas dentro de `tags` (ex: `author:`, `dominio:`, `Subdominio:`, `tipo:`, `up:`, `same:`, `down:`, `source:`, `description:`) — problema identificado na auditoria dos 515 arquivos de Livros/Métodos (ver seção 14.7).
7. Remover nomes de autores de dentro de `tags` — autores vão só no campo `autor` (fonte) ou `autores_relacionados` (conceito).

_(Ferramenta sugerida: plugin Bulk Tag Manager no Obsidian, via BRAT.)_

### Passo 2 — Os 4 tipos de nota
Ver seção 4. Arquivos "HUB" (Business HUB, Marketing HUB, Branding HUB, IA HUB) já são do tipo `moc` — só precisam ganhar os campos, sem mudar de nome nem de conteúdo.

### Passo 3 — Hierarquia de departamentos
Ver seção 3. Escrever por extenso a árvore completa antes de usar os slugs no frontmatter.

### Passo 4 — Ativar os campos nos HUBs existentes
1. `Business HUB.md` → adiciona `tipo: moc`, `dominio: business`, `departamentos: [...]`.
2. `Marketing HUB.md`, `Branding HUB.md`, `IA HUB.md`, `Vendas HUB.md`, `Finanças HUB.md` → adicionam `tipo: moc`, `dominio: <departamento>`, e (quando fizer sentido) `autoridade_padrao`.

Usar o `Branding HUB.md` como teste do formato inteiro (autoridade Sharp/Romaniuk → Holt → Aaker/Kotler → Neumeier) antes de replicar pros outros HUBs.

### Passo 5 — Marcar tier e status nas fontes existentes
Pegar as notas de `fonte` já existentes dentro de um domínio (ex: Branding: Sharp, Holt, Aaker, Kotler, Neumeier) e adicionar os campos da seção 4.2. Usar `status: contestado` nas que já se sabe que divergem entre si.

### Passo 6 — Escrever a primeira nota `sintese`
Escolher um conflito real já resolvido (ex: posicionamento vs. saliência) e escrever como nota formal, usando o modelo da seção 4.4.

### Passo 7 — Replicar pros outros departamentos
Só depois que o ciclo completo (HUB com autoridade → fontes com tier/status → pelo menos uma síntese) funcionar bem em um departamento, repetir nos outros (Marketing, IA, Business, Vendas, Finanças).

### Passo 8 — Preencher notas sem frontmatter nenhum
Os 30 arquivos sem metadado (incluindo artigos-chave do Projeto Prometheus) são um bom lote pra praticar o schema desde o zero. Ver também seção 14.7 pro lote maior de 515 arquivos de Livros/Métodos.

### Passo 9 — Popular `agente` e `setor` (opcional, ver seção 14)
Depois que o schema base (Passos 0-8) estiver estável, adicionar a camada de roteamento RAG multiagente — começar pelos MOCs de domínio (`agente: ceo/cmo/cbo/cto/...`) e só depois descer pras notas operacionais (`setor`).

---

## 12. Checklist de progresso

- [ ] Passo 0 — branch de backup criada
- [ ] Passo 1 — tags mecânicas corrigidas
- [ ] Passo 2 — os 4 tipos entendidos (moc/fonte/conceito/sintese)
- [ ] Passo 3 — árvore de departamentos escrita por extenso
- [ ] Passo 4 — HUBs existentes com campos novos (começando por Branding)
- [ ] Passo 5 — fontes de um departamento com tier/status
- [ ] Passo 6 — primeira síntese escrita
- [ ] Passo 7 — replicado em outro departamento
- [ ] Passo 8 — arquivos órfãos preenchidos
- [ ] Passo 9 — agente/setor populados nos MOCs de domínio

---

## 13. Notas de implementação

- O plugin **Breadcrumbs** apenas renderiza (trilhas, Matrix view, grafo lateral) o que já está escrito no YAML. Um agente de IA lendo o `.md` bruto acessa `up/down/same` diretamente, sem depender do plugin instalado.
- O campo `nivel` existe pra permitir queries via Dataview (ex: listar todas as notas operacionais de um domínio) independente da navegação relacional do Breadcrumbs.
- Escrever os campos manualmente ou via template é preferível a depender de inferência automática do plugin baseada em estrutura de pastas, pra garantir que o dado sobrevive independente da ferramenta.
- Quando o usuário enviar um link do GitHub do vault, checar: (1) se os arquivos seguem esse esquema de metadata, (2) apontar divergências específicas e o que falta, (3) gerar feedback de melhorias.

---

## 14. Sistema de agentes de IA (roteamento RAG multiagente)

Camada incorporada da proposta opencode, adaptada ao princípio fractal (6.9) e à estrutura já existente (dominio em vez de departamento, tipo mantido como está).

### 14.1 Princípio
Cada nota pode ser rotulada com o **agente dono** (`agente`). Um agente consulta o RAG com escopo restrito ao seu departamento/setor (filtros sobre `nivel`, `dominio`, `setor`). Agentes de nível superior comandam os inferiores:
- **Informação (subindo):** o superior consulta o escopo do subordinado quando precisa de detalhe.
- **Execução (descendo):** o superior roteia a tarefa pro agente do setor correto.
- A cadeia de comando é definida **uma única vez**, no organograma abaixo — nunca repetida por documento.
- **Recursividade fractal:** a cadeia não para em 3 camadas fixas (ceo → c-level → setor). Qualquer nó com complexidade suficiente pode ganhar seu próprio agente, subordinado ao agente do nível acima — ex: `cpo` dentro do microverso de Produto, subordinado ao `cmo`.

### 14.2 Organograma de agentes

| Agente | Nível (visto do pai imediato) | Dominio/escopo | Reporta para | Comanda |
|---|---|---|---|---|
| `ceo` | estrategico (raiz) | `business` (todos os documentos) | — | `cmo`, `cbo`, `cto`, `cso`, `cfo` |
| `cmo` | tatico (visão do ceo) / estrategico (microverso marketing) | `marketing` | `ceo` | `copywriting`, `seo`, `precificacao`, `promocao`, `praca`, `funil-de-vendas`, `segmentacao`, `email-marketing`, `marketing-de-conteudo`, `storytelling`, `canais`, `cpo` (fractal) |
| `cbo` | tatico / estrategico (microverso branding) | `branding` | `ceo` | `posicionamento`, `naming`, `marca`, `valores` |
| `cto` | tatico / estrategico (microverso IA) | `inteligenciaartificial` | `ceo` | setores de IA (a definir) |
| `cso` | tatico / estrategico (microverso vendas) | `vendas` | `ceo` | setores de vendas (a definir) |
| `cfo` | tatico / estrategico (microverso finanças) | `financas` | `ceo` | setores de finanças (a definir) |
| `cpo` (fractal) | tatico (visão do cmo) / operacional (visão do ceo) / estrategico (microverso produto) | `produto`, dentro de `marketing` | `cmo` | setores de produto (a definir) |
| `copywriting`, `seo`, `precificacao`, `naming`, … | operacional | `setor = <agente>` | C-Level do seu departamento | — |

**Correção de fusão (ver 6.10):** na proposta original, `cpo` reportava direto ao `ceo`, no mesmo nível de `cmo`/`cbo`. Corrigido pra reportar ao `cmo`, respeitando o princípio fractal — Produto é tático dentro do microverso de Marketing.

### 14.3 Vocabulário de `agente`
`ceo` · `cmo` · `cbo` · `cto` · `cso` · `cfo` · `cpo` · `copywriting` · `seo` · `precificacao` · `promocao` · `praca` · `funil-de-vendas` · `segmentacao` · `email-marketing` · `marketing-de-conteudo` · `storytelling` · `canais` · `posicionamento` · `naming` · `marca` · `valores` · `estrategia` · `inovacao`

> Novo agente => registrar no organograma (14.2) + adicionar slug ao vocabulário.

### 14.4 Regras de roteamento RAG (templates de query)

| Agente | Query de roteamento |
|---|---|
| `ceo` | `(nivel = estrategico) OR (todo escopo dos subordinados)` |
| `cmo` | `dominio = marketing AND nivel >= tatico` (ou delega `setor` pro operacional) |
| `cbo` | `dominio = branding` |
| `cto` / `cso` / `cfo` | `dominio = inteligenciaartificial` / `vendas` / `financas` |
| `cpo` | `dominio = marketing AND subdominio LIKE "marketing-tatico-produto%"` |
| setor `x` | `dominio = <pai> AND setor = x` |

### 14.5 Vocabulário controlado de `setor`
`copywriting` · `precificacao` · `promocao` · `praca` · `produto` · `funil-de-vendas` · `segmentacao` · `estrategia` · `posicionamento` · `naming` · `storytelling` · `email-marketing` · `marketing-de-conteudo` · `seo` · `canais` · `marca` · `inovacao` · `valores`

### 14.6 Campo `tipo` — eixo mantido (ver 4, 6.10)
O eixo epistêmico (`moc`/`fonte`/`conceito`/`sintese`) foi mantido como está. A granularidade estrutural que a proposta opencode usava (`livro`/`capítulo`/`método`/`exemplo`/`case`/`guia`) não entrou como substituto — pode ser considerada como campo adicional (`formato`, por exemplo) numa fase futura, se necessário, sem mexer no `tipo`.

### 14.7 Escopo-piloto: 515 arquivos de Livros & Métodos
A proposta opencode foi construída em cima de uma auditoria real de `3. Recursos/Estudos/Livros` (415 arquivos) + `3. Recursos/Estudos/Métodos` (100 arquivos). Problemas identificados nesse lote, úteis pra guiar a limpeza (ver Passo 1):
- Autores dentro de `tags` (deveriam estar em `autor`/`autores_relacionados`).
- Chaves de metadata vazadas em `tags` (`author:`, `dominio:`, `tipo:`, `up:`…).
- Acentos/case inconsistentes (`marketing-estrategico` vs `marketing-tático`).
- ~360 arquivos sem hierarquia nenhuma (só `tags` + `author`) — inválidos pra roteamento por agente até serem preenchidos.

**Execução sugerida (não executado ainda):**
1. Script (ex: PowerShell) lê os frontmatters (UTF-8 explícito), calcula `tipo`, `nivel`, `agente`, `dominio`, `setor`, `tags` limpas pelas regras acima, e reescreve só o frontmatter — corpo do arquivo intacto.
2. Revisão via `git diff --stat` + amostragem por `tipo` e conferência de `agente`.
3. Criar a nota de taxonomia `3. Recursos/Estudos/Taxonomia de Metadata.md`, com o organograma (14.2), definição de campos, vocabulários controlados e templates de roteamento (14.4) — fonte única de consulta pro sistema de agentes.
4. Sem commit automático, a menos que solicitado.

**Fase 2 (opcional):** popular `up`/`down` do Breadcrumbs em massa pra navegação livro→capítulo — não confundir com a cadeia de comando de agentes, que vive só no organograma (14.2).
