---
tipo:
  - sintese
dominio:
  - IA
Subdominio:
  - RAG
---
# Guia Mestre — Metadados do Second Brain

> Documento de referência para uso por agentes de IA (Claude) como contexto de consulta. Consolida a estrutura hierárquica, os tipos de nota, os campos de metadata, as decisões de design e o roteiro de implementação do vault. Este documento substitui/consolida as versões anteriores — nenhuma decisão antiga foi removida, apenas atualizada ou complementada (ver seção 6 para o histórico de mudanças).

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

## 3. Hierarquia de departamentos (Business como raiz)

**Atualização de estrutura:** Business é tratado como departamento-raiz administrativo ("CEO"), com Marketing, Branding, Inteligência Artificial (e outros) subordinados abaixo dele. **Isso substitui a definição anterior, em que os três domínios eram tratados como irmãos sem hierarquia** (ver histórico em 6.5).

```
business (CEO)
  marketing
    marketing-estrategico
    marketing-tatico
      marketing-operacional
  branding
    branding-estrategico
      branding-tatico-preco
      branding-tatico-produto
      branding-tatico-promocao
        branding-operacional-promocao-copywriting
      branding-tatico-praca-canais
        branding-operacional-canal-blog
  inteligenciaartificial
```

**Antes de usar os slugs no frontmatter**, escreva por extenso a árvore completa dos seus departamentos (igual ao exemplo acima), pra fixar os nomes exatos e evitar variações tipo `branding-tatico-preço` numa nota e `branding-tatico-preco` (sem acento) em outra.

Essa hierarquia administrativa é documentada **uma vez só**, no MOC raiz (`Business HUB.md`), via campo `departamentos`. Não existe campo `dominio_raiz` ou `fluxo` repetido em cada nota individual — um campo que sempre vale "business" em toda nota não discrimina nada no retrieval e só custa tokens à toa (ver 6.7).

Dentro de cada departamento, a relação `up/down/same` do Breadcrumbs continua descrevendo a estrutura **interna** do próprio domínio (ex: Business → seus 9 blocos do BMC; Marketing → seus 4 Ps; Branding → suas 4 categorias de identidade) — não a relação entre departamentos em si, que fica centralizada no campo `departamentos`. Se quiser que o Breadcrumbs também renderize visualmente o vínculo Marketing/Branding → Business, basta adicionar `up: "[[Business]]"` nas notas-mãe de cada departamento — isso é opcional e complementar.

### 3.1 Categorias táticas fixas por domínio

|Domínio|Categorias táticas (fixas)|
|---|---|
|**Marketing**|Preço, Praça, Produto, Promoção|
|**Business**|Proposta de Valor, Segmentos de Cliente, Canais, Relacionamento com Cliente, Fontes de Receita, Estrutura de Custos, Recursos-Chave, Atividades-Chave, Parcerias-Chave (9 blocos do Business Model Canvas)|
|**Branding**|Identidade Verbal, Identidade Visual, Personalidade de Marca, Experiência de Marca|
|**Inteligência Artificial**|Ainda não definidas|

---

## 4. Os 4 tipos de nota

Toda nota do vault se encaixa em um destes 4 tipos, identificado pelo campo `tipo`. Arquivos "HUB" (Business HUB, Marketing HUB, Branding HUB, IA HUB) são do tipo `moc` — só precisam ganhar os campos abaixo, sem mudar de nome nem de conteúdo.

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

|Campo|Tipo|Função|Onde aparece|
|---|---|---|---|
|`tags`|lista|Tag aninhada nativa do Obsidian, ex: `dominio/business`, `nivel/tatico`|toda nota|
|`nivel`|texto|Um de: `estrategico`, `tatico`, `operacional` — filtro simples via Dataview|toda nota|
|`dominio`|texto|Departamento: `business`, `marketing`, `branding`, `inteligenciaartificial`|toda nota|
|`subdominio`|texto (slug)|Nível tático dentro do departamento, formato `dominio-nivel` (ex: `branding-estrategico`) — granularidade extra, **complementar** ao `nivel`, não o substitui|`fonte`, `conceito`, `sintese`|
|`sub_subdominio`|texto (slug)|Nível operacional, mais específico (ex: `branding-operacional-promocao-copywriting`) — opcional, deixe vazio se não usar|`fonte`, `conceito`, `sintese` (opcional)|
|`up`|link|Nota-pai (Breadcrumbs)|toda nota|
|`down`|lista de links|Notas-filhas (Breadcrumbs)|toda nota|
|`same`|lista de links|Notas laterais / relacionadas, mesmo nível (Breadcrumbs)|toda nota|
|`tipo`|texto|`moc` / `fonte` / `conceito` / `sintese`|toda nota|
|`autor`|texto|Autor da fonte|só `fonte`|
|`obra`|texto|Livro/capítulo de origem|só `fonte`|
|`tier`|número|`0` = síntese própria · `1` = referência curada · `2` = rascunho/nota bruta|`fonte`, `conceito`, `sintese`|
|`status`|texto|`consenso` / `contestado` / `sua_posicao_atual`|`fonte`, `conceito`, `sintese`|
|`autores_relacionados`|lista|Autores que tocam nesse conceito|só `conceito`|
|`resolve_conflito_entre`|lista|Autores/fontes que a síntese resolve|só `sintese`|
|`autoridade_padrao`|lista ordenada|Ordem de desempate entre autores do domínio|só `moc` de domínio|
|`departamentos`|lista|Departamentos subordinados (só no MOC raiz de Business)|só `moc` raiz (Business HUB)|

Campo `escola` (pra marcar teoria de origem, ex: Sharp vs. Aaker vs. Kotler) foi **avaliado e descartado** — decisão de manter estrutura departamental fixa em vez de marcação por escola teórica em toda nota (ver 6.3).

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

Decisão original: Marketing, Business e Branding eram tratados como **irmãos** entre si (relação `same`), sem hierarquia entre eles. **Essa decisão foi revista e substituída**: Business passou a ser tratado como departamento-raiz administrativo ("CEO"), com Marketing, Branding e Inteligência Artificial subordinados a ele (ver seção 3). Fica registrado aqui para histórico — a estrutura vigente é a da seção 3.

### 6.6 Campo `nivel` mantido junto com `subdominio`/`sub_subdominio`

Avaliado substituir `nivel` pelos slugs compostos (`subdominio`/`sub_subdominio`), mas decidido **manter os dois**: `nivel` continua como campo simples (`estrategico`/`tatico`/`operacional`) pra filtros rápidos via Dataview; `subdominio`/`sub_subdominio` são campos extras que adicionam granularidade (departamento + nível combinados num slug), úteis pra notas `fonte`/`conceito`/`sintese`. Não há redundância eliminada — são dois níveis de granularidade coexistindo.

### 6.7 Sem campo `dominio_raiz` ou `fluxo`

Um campo que sempre vale "business" em toda nota não discrimina nada no retrieval — só custa tokens à toa. A relação de raiz administrativa (Business → Marketing/Branding/IA) fica documentada uma única vez, no campo `departamentos` do MOC raiz (`Business HUB.md`).

---

## 7. Templates de frontmatter por tipo de nota

Ver exemplos completos em `moc`/`fonte`/`conceito`/`sintese` na seção 4. Esses campos combinam com os templates por nível hierárquico abaixo (seção 8) — uma nota real normalmente carrega os dois conjuntos de campos juntos (ver exemplo combinado, seção 9).

## 8. Templates de frontmatter por nível hierárquico (Breadcrumbs)

### 8.1 Nota-mãe (Estratégico)

```yaml
---
tags:
  - dominio/<business|marketing|branding|inteligenciaartificial>
  - nivel/estrategico
nivel: estrategico
dominio: <business|marketing|branding|inteligenciaartificial>
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
  - dominio/<business|marketing|branding|inteligenciaartificial>
  - nivel/tatico
nivel: tatico
dominio: <business|marketing|branding|inteligenciaartificial>
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
  - dominio/<business|marketing|branding|inteligenciaartificial>
  - nivel/operacional
nivel: operacional
dominio: <business|marketing|branding|inteligenciaartificial>
up: "[[<Nota tática correspondente>]]"
same:
  - "[[<ferramenta operacional vizinha 1>]]"
  - "[[<ferramenta operacional vizinha 2>]]"
---
```

---

## 9. Exemplo combinado completo

Uma nota `fonte` real, unindo os dois sistemas de campos (tipo/tier/status + up/down/same):

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

_(Ferramenta sugerida: plugin Bulk Tag Manager no Obsidian, via BRAT.)_

### Passo 2 — Os 4 tipos de nota

Ver seção 4. Arquivos "HUB" (Business HUB, Marketing HUB, Branding HUB, IA HUB) já são do tipo `moc` — só precisam ganhar os campos, sem mudar de nome nem de conteúdo.

### Passo 3 — Hierarquia de departamentos

Ver seção 3. Escrever por extenso a árvore completa antes de usar os slugs no frontmatter.

### Passo 4 — Ativar os campos nos HUBs existentes

1. `Business HUB.md` → adiciona `tipo: moc`, `dominio: business`, `departamentos: [...]`.
2. `Marketing HUB.md`, `Branding HUB.md`, `IA HUB.md` → adicionam `tipo: moc`, `dominio: <departamento>`, e (quando fizer sentido) `autoridade_padrao`.

Usar o `Branding HUB.md` como teste do formato inteiro (autoridade Sharp/Romaniuk → Holt → Aaker/Kotler → Neumeier) antes de replicar pros outros HUBs.

### Passo 5 — Marcar tier e status nas fontes existentes

Pegar as notas de `fonte` já existentes dentro de um domínio (ex: Branding: Sharp, Holt, Aaker, Kotler, Neumeier) e adicionar os campos da seção 4.2. Usar `status: contestado` nas que já se sabe que divergem entre si.

### Passo 6 — Escrever a primeira nota `sintese`

Escolher um conflito real já resolvido (ex: posicionamento vs. saliência) e escrever como nota formal, usando o modelo da seção 4.4.

### Passo 7 — Replicar pros outros departamentos

Só depois que o ciclo completo (HUB com autoridade → fontes com tier/status → pelo menos uma síntese) funcionar bem em um departamento, repetir nos outros (Marketing, IA, Business).

### Passo 8 — Preencher notas sem frontmatter nenhum

Os 30 arquivos sem metadado (incluindo artigos-chave do Projeto Prometheus) são um bom lote pra praticar o schema desde o zero.

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

---

## 13. Notas de implementação

- O plugin **Breadcrumbs** apenas renderiza (trilhas, Matrix view, grafo lateral) o que já está escrito no YAML. Um agente de IA lendo o `.md` bruto acessa `up/down/same` diretamente, sem depender do plugin instalado.
- O campo `nivel` existe pra permitir queries via Dataview (ex: listar todas as notas operacionais de um domínio) independente da navegação relacional do Breadcrumbs.
- Escrever os campos manualmente ou via template é preferível a depender de inferência automática do plugin baseada em estrutura de pastas, pra garantir que o dado sobrevive independente da ferramenta.
- Quando o usuário enviar um link do GitHub do vault, checar: (1) se os arquivos seguem esse esquema de metadata, (2) apontar divergências específicas e o que falta, (3) gerar feedback de melhorias.