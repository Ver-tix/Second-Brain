---
tipo:
  - sintese
dominio:
  - IA
Subdominio:
  - RAG
---
# Guia Mestre — Metadados do Second Brain

### Documento único e completo · Substitui/consolida os documentos anteriores

### Second Brain · Subprojeto de Economia de Tokens · Prometheus

Este documento junta tudo: os tipos de nota, os campos, a hierarquia de departamentos e os passos de implementação — cada passo já vem com as opções completas dentro dele, sem precisar consultar outro arquivo.

---

## Passo 0 — Backup de segurança

1. Confirme que seu vault está com todos os commits em dia no GitHub.
2. Crie uma branch nova pra essa migração: `git checkout -b reforma-metadados`.

---

## Passo 1 — Limpeza básica de tags

Antes de introduzir o schema novo, arrume a bagunça atual (baseado na auditoria já feita):

1. **Remover o `#` de dentro do campo tags** (6 arquivos): `#estratégia`, `#Funding`, `#descoberta`, `#dicionario`, `#inteligenciaartificial`, `#tecnologia`.
2. **Corrigir o typo**: `brandig` → `branding`.
3. **Unificar duplicatas singular/plural**: `case`/`cases`, `funil`/`funis`, `tático`/`táticas` — escolha uma forma pra cada.
4. **Padronizar capitalização**: decida lowercase geral e ajuste `BrandEquity`, `RealEstate`, `Funding`, `Prompts`, `RAG`.
5. **Revisar as 6 tags órfãs restantes**: `brandDesign`, `comportamental`, `instruction`, `luxo`, `segmentação`, `vendas`.

_(Ferramenta sugerida pra isso: plugin Bulk Tag Manager no Obsidian, via BRAT — já cobrimos como instalar e usar.)_

---

## Passo 2 — Os 4 tipos de nota (com todos os campos)

Toda nota do vault se encaixa em um destes 4 tipos, identificado pelo campo `tipo`. **Seus arquivos "HUB" (Business HUB, Marketing HUB, Branding HUB, IA HUB) já são o tipo `moc`** — só precisam ganhar os campos abaixo, sem mudar de nome nem de conteúdo.

### 2.1 `moc` (os seus "HUB")

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

**Campo `autoridade_padrao`:** só existe nos MOCs de domínio (não em `fonte`/`conceito`/`sintese`). Define a ordem de desempate quando há divergência entre autores e você não especificou de quem quer a visão.

### 2.2 `fonte`

Uma nota sobre um livro, capítulo ou autor específico — a maioria das suas notas atuais.

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

### 2.3 `conceito`

Uma ideia que atravessa várias fontes/autores diferentes, com ângulos distintos.

```yaml
tipo: conceito
dominio: branding
subdominio: branding-estrategico
autores_relacionados: ["Byron Sharp", "Jenni Romaniuk", "Douglas Holt"]
tier: 1
status: contestado
```

### 2.4 `sintese`

Sua posição própria, já resolvida sobre um conflito entre fontes. É Tier 0 — a nota mais valiosa pro RAG.

```yaml
tipo: sintese
dominio: branding
subdominio: branding-estrategico
tier: 0
status: sua_posicao_atual
resolve_conflito_entre: ["Byron Sharp", "Douglas Holt"]
```

### Tabela-resumo de quando usar cada tipo

|Situação da nota|Tipo|
|---|---|
|É um HUB que organiza e aponta pra outras notas|`moc`|
|É sobre um livro/autor/capítulo específico|`fonte`|
|É uma ideia que aparece em várias fontes diferentes|`conceito`|
|É a sua posição própria sobre algo que já debateu|`sintese`|

### Explicação de cada campo (glossário rápido)

|Campo|O que significa|Onde aparece|
|---|---|---|
|`tipo`|Qual dos 4 tipos acima|Toda nota|
|`dominio`|Departamento (Business, Marketing, Branding, IA...)|Toda nota|
|`subdominio`|Nível tático dentro do departamento|`fonte`, `conceito`, `sintese`|
|`sub_subdominio`|Nível operacional, mais específico|`fonte`, `conceito`, `sintese` (opcional, deixe vazio se não usar)|
|`tier`|0 = sua síntese · 1 = referência curada · 2 = rascunho/nota bruta|`fonte`, `conceito`, `sintese`|
|`status`|`consenso` / `contestado` / `sua_posicao_atual`|`fonte`, `conceito`, `sintese`|
|`autor` / `obra`|Identifica a fonte|Só em `fonte`|
|`autores_relacionados`|Lista de autores que tocam nesse conceito|Só em `conceito`|
|`resolve_conflito_entre`|Quais autores/fontes essa síntese resolve|Só em `sintese`|
|`autoridade_padrao`|Ordem de desempate do domínio|Só em `moc`|

---

## Passo 3 — A hierarquia de departamentos (Business como raiz)

Sua estrutura é uma **departamentalização administrativa**: Business é o "CEO", e abaixo dele, em paralelo, ficam Marketing, Branding, IA e outros departamentos. Abaixo de cada departamento, as partes táticas/operacionais.

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

**Decisão importante: não existe campo `dominio_raiz` ou `fluxo`.** Um campo que sempre vale "business" em toda nota não discrimina nada no retrieval — só custa tokens à toa. Em vez disso:

- O `Business HUB.md` (seu MOC raiz) documenta essa árvore inteira **uma vez só**:

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

- Os campos `dominio` / `subdominio` / `sub_subdominio` de cada nota individual **continuam exatamente como definidos no Passo 2** — eles já cobrem o estreitamento tático → operacional dentro de cada departamento.

**Antes de usar os slugs no frontmatter**, escreva por extenso a árvore completa dos seus departamentos (igual ao exemplo acima), pra fixar os nomes exatos e evitar variações tipo `branding-tatico-preço` numa nota e `branding-tatico-preco` (sem acento) em outra.

---

## Passo 4 — Ativar os campos nos HUBs existentes

Não crie nada novo — só adicione os campos do Passo 2 aos arquivos HUB que já existem:

1. `Business HUB.md` → adiciona `tipo: moc`, `dominio: business`, `departamentos: [...]` (modelo do Passo 3).
2. `Marketing HUB.md`, `Branding HUB.md`, `IA HUB.md` → adicionam `tipo: moc`, `dominio: <departamento>`, e (quando fizer sentido) `autoridade_padrao`.

Use o `Branding HUB.md` como teste do formato inteiro (autoridade Sharp/Romaniuk → Holt → Aaker/Kotler → Neumeier) antes de replicar pros outros HUBs.

---

## Passo 5 — Marcar tier e status nas fontes existentes

Pegue as notas de `fonte` já existentes dentro de um domínio (ex: Branding: Sharp, Holt, Aaker, Kotler, Neumeier) e adicione os campos do Passo 2.2. Use `status: contestado` nas que você já sabe que divergem entre si.

---

## Passo 6 — Escrever sua primeira nota `sintese`

Escolha um conflito real que você já resolveu na cabeça (ex: posicionamento vs. saliência) e escreva como nota formal, usando o modelo do Passo 2.4.

---

## Passo 7 — Replicar pros outros departamentos

Só depois que o ciclo completo (HUB com autoridade → fontes com tier/status → pelo menos uma síntese) funcionar bem em um departamento, repita nos outros (Marketing, IA, Business).

---

## Passo 8 — Preencher notas sem frontmatter nenhum

Os 30 arquivos sem metadado (incluindo artigos-chave do Projeto Prometheus) são um bom lote pra praticar o schema desde o zero.

---

## Checklist de progresso

- [ ] Passo 0 — branch de backup criada
- [ ] Passo 1 — tags mecânicas corrigidas
- [ ] Passo 2 — os 4 tipos entendidos (moc/fonte/conceito/sintese)
- [ ] Passo 3 — árvore de departamentos escrita por extenso
- [ ] Passo 4 — HUBs existentes com campos novos (começando por Branding)
- [ ] Passo 5 — fontes de um departamento com tier/status
- [ ] Passo 6 — primeira síntese escrita
- [ ] Passo 7 — replicado em outro departamento
- [ ] Passo 8 — arquivos órfãos preenchidos