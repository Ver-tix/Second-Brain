# Proposta: Sistema de Metadata Departamental para RAG de Agentes — Livros & Métodos

**Autor:** opencode · **Data:** 05/08/2026 · **Escopo:** `3. Recursos/Estudos/Livros` (415 arquivos) + `3. Recursos/Estudos/Métodos` (100 arquivos) = **515 arquivos**

---

## 0. Finalidade do sistema

A metadata organiza o conteúdo para **RAG (Retrieval-Augmented Generation)** de um **ecossistema de agentes de IA**, um agente por nível departamental:

- Cada documento é rotulado com o **agente dono** (`agente`).
- Cada agente consulta o RAG com **escopo restrito** ao seu departamento/setor (filtros sobre `nivel`, `departamento`, `setor`).
- **Agentes de departamentos superiores comandam os inferiores:**
  - para **informação**, consultam o escopo do subordinado;
  - para **execução**, roteiam tarefas para o agente subordinado.
- A **cadeia de comando** é definida uma única vez (organograma na taxonomia), nunca repetida por documento.

---

## 1. Contexto atual

Todos os 515 arquivos possuem frontmatter:

| Campo | Ocorrências | Observação |
|---|---|---|
| `tags` | 513 | Com autores, chaves vazadas e acentos inconsistentes |
| `author` | 216 | Correto, mantido |
| `dominio` | 153 | `marketing`, `branding`, `business e administração` |
| `Subdominio` | 148 | Mistura departamento + nível (ex.: `marketing-tático-mix`) |
| `tipo` | 136 | Apenas `MOC` e `fonte` |
| `Sub_subdominio` | 133 | Quase equivalente a "setor" |
| `description` | 15 | Mantido como campo livre |
| `up` / `same` / `down` | 11 / 9 / 1 | Campos do plugin Breadcrumbs (navegação doc↔doc) |
| `source` | 3 | URLs, mantido |

### Problemas identificados
1. Autores dentro de `tags` (Russell Brunson, Gabriel Weinberg, Robert W. Bly, Philip Kotler…).
2. Chaves de metadata vazadas em `tags` (`author:`, `dominio:`, `tipo:`, `up:`…).
3. `tipo` insuficiente — não distingue `livro`, `capítulo`, `método`, `exemplo`, `case`, `guia`.
4. Acentos/case inconsistentes (`marketing-estrategico` vs `marketing-tático`; `analise-mercados`).
5. ~360 arquivos sem hierarquia (só `tags` + `author`) — **inválidos para roteamento por agente**.
6. Nomeação confusa — `Subdominio`/`Sub_subdominio` misturam eixo de nível e eixo departamental.

---

## 2. Novo esquema YAML

```yaml
---
tipo: capítulo         # livro | capítulo | método | MOC | fonte | exemplo | case | guia
nivel: operacional     # estratégico | tático | operacional
agente: copywriting    # dono do conteúdo (chave primária do RAG)
departamento: marketing  # business | marketing | branding | produto | vendas | finanças
setor: copywriting     # setor funcional (operacional)
tags:
  - copywriting
author:
  - Robert W. Bly
source: https://...    # mantém quando existir
description: "..."     # mantém quando existir
up: "[[...]]"          # Breadcrumbs — navegação doc↔doc (não é cadeia de comando)
same: [...]
down: [...]
---
```

### Modelo conceitual — dois eixos + dono
- **Eixo hierárquico (`nivel`):** `estratégico` (CEO/C-Suite) > `tático` (departamentos C-Level) > `operacional` (setores).
- **Eixo departamental (`departamento`):** quem é "dono" na organização (CMO=marketing, CBO=branding…).
- **`agente`:** nome do agente responsável — derivável do organograma, mas gravado explicitamente para roteamento RAG rápido e confiável.

**Exemplos:**
- `How Brands Grow` → `nivel: estratégico` + `agente: cmo` + `departamento: marketing`
- `Competitive Advantage` → `nivel: estratégico` + `agente: ceo` + `departamento: business`
- `Como Escrever Uma Copy Que Vende` → `nivel: operacional` + `agente: copywriting` + `departamento: marketing` + `setor: copywriting`
- `Balanced Scorecard` → `nivel: estratégico` + `agente: ceo` + `departamento: business` + `setor: estratégia`

---

## 3. Organograma de agentes (cadeia de comando)

A cadeia de comando vive **na taxonomia (config)**, não nos documentos. Cada doc declara apenas `agente`.

| Agente | Nível | Escopo RAG (filtro) | Comanda | Respondido por |
|---|---|---|---|---|
| `ceo` | estratégico | todos os documentos | `cmo`, `cbo`, `cpo`, `cso`, `cfo` | — |
| `cmo` | tático | `departamento = marketing` | `copywriting`, `seo`, `precificação`, `promoção`, `praça`, `funil-de-vendas`, `segmentação`, `e-mail-marketing`, `marketing-de-conteúdo`, `storytelling`, `canais` | `ceo` |
| `cbo` | tático | `departamento = branding` | `posicionamento`, `naming`, `marca`, `valores` | `ceo` |
| `cpo` | tático | `departamento = produto` | setores de produto | `ceo` |
| `cso` | tático | `departamento = vendas` | — | `ceo` |
| `cfo` | tático | `departamento = finanças` | — | `ceo` |
| `copywriting`, `seo`, `precificação`, `naming`, … | operacional | `setor = <agente>` | — | C-Level do seu departamento |

**Mecanismo de comando:**
- **Informação (subindo):** o superior consulta o escopo do subordinado (`setor = x`) quando precisa de detalhe.
- **Execução (descendo):** o superior roteia a tarefa para o agente do setor correto via organograma.
- A ordem de comando respeita `nivel`: estratégico (3) > tático (2) > operacional (1); nunca há comando entre departamentos irmãos.

### 3.1 Vocabulário de `agente`
`ceo` · `cmo` · `cbo` · `cpo` · `cso` · `cfo` · `copywriting` · `seo` · `precificação` · `promoção` · `praça` · `funil-de-vendas` · `segmentação` · `e-mail-marketing` · `marketing-de-conteúdo` · `storytelling` · `canais` · `posicionamento` · `naming` · `marca` · `valores` · `estratégia` · `inovação`

> Novo agente => registrar no organograma da taxonomia + adicionar slug ao vocabulário.

---

## 4. Regras de roteamento RAG (templates de query)

| Agente | Query de roteamento |
|---|---|
| `ceo` | `(nivel = estratégico) OR (todo escopo dos subordinados)` |
| `cmo` | `departamento = marketing AND nivel >= tático` (ou delega `setor` para operacional) |
| `cbo` | `departamento = branding` |
| `cpo` / `cso` / `cfo` | `departamento = produto` / `vendas` / `finanças` |
| setor `x` | `departamento = <pai> AND setor = x` |

---

## 5. Vocabulários controlados

### `tipo`
`livro` · `capítulo` · `método` · `MOC` · `fonte` · `exemplo` · `case` · `guia`

### `nivel`
`estratégico` · `tático` · `operacional`

### `departamento`
`business` (CEO) · `marketing` (CMO) · `branding` (CBO) · `produto` (CPO) · `vendas` (CSO) · `finanças` (CFO)

### `setor`
`copywriting` · `precificação` · `promoção` · `praça` · `produto` · `funil-de-vendas` · `segmentação` · `estratégia` · `posicionamento` · `naming` · `storytelling` · `e-mail-marketing` · `marketing-de-conteúdo` · `SEO` · `canais` · `marca` · `inovação` · `valores`

---

## 6. Regras de migração

### 6.1 `dominio` → `departamento`
| Atual | Novo |
|---|---|
| `marketing` | `departamento: marketing` |
| `branding` | `departamento: branding` |
| `business e administração` | `departamento: business` |

### 6.2 `Subdominio` → `nivel` (+ `setor`)
| Atual | Nível | Setor |
|---|---|---|
| `marketing-estrategico` | `estratégico` | `estratégia` |
| `marketing-tático` | `tático` | — |
| `marketing-operacional` | `operacional` | — |
| `marketing-tático-funil` | `tático` | `funil-de-vendas` |
| `marketing-tático-mix` | `tático` | — |
| `branding-posicionamento` | `tático` | `posicionamento` |
| `branding-pessoal` | `tático` | `branding-pessoal` |
| `segmentos-de-cliente` | `tático` | `segmentação` |

### 6.3 `Sub_subdominio` → `setor`
`copywriting`→`copywriting` · `precificação`→`precificação` · `promoção`→`promoção` · `praça`→`praça` · `produto`→`produto` · `segmentação-cliente`/`segmentação-mercado`→`segmentação` · `funil-de-vendas`→`funil-de-vendas` · `planejamento/análise/diagnóstico-estratégico`→`estratégia` · `ações-estruturantes`→`estratégia` · `slogan`→`naming` · `valores`→`valores` · `analise-mercados`→`estratégia` · `e-mail_marketing`→`e-mail-marketing`

### 6.4 `agente` (derivado de `nivel`+`departamento`+`setor`)
- `nivel: estratégico` + `departamento: business` → `ceo`
- `nivel: tático` + `departamento: X` → `cmo` / `cbo` / `cpo` / `cso` / `cfo`
- `nivel: operacional` + `setor: S` → slug do setor (`copywriting`, `seo`, …), com pai = C-Level do departamento

### 6.5 Classificação dos ~360 arquivos sem hierarquia (pasta + nome)
**Livro → departamento (24):**
- `marketing`: Administração em Marketing, Como Escrever Uma Copy Que Vende, DotCom Secrets, Expert Secrets, Manual do Copywriter, Marketing Canvas, O Plano de Marketing de 1 Página, Storybrand, Tração
- `branding`: Building Distinctive Assets, Building Strong Brands, How Brands Become Icons, How Brands Grow, How Brands Grow 2, Logica do Consumo, Managing Brand Equity, O Herói e o Fora-da-Lei, O Poder do Naming, Posicionamento, THE BRAND GAP, ZAG
- `business`: Competitive Advantage, Estratégia do Oceano Azul, Monetizing Innovation, Crédito Vale Mais do Que Dinheiro

**Método → departamento (10):**
- `business`: 10 Tipos de Inovação, Balanced Scorecard, Business Model Canvas, V.R.I.O Framework, Value Proposition Design
- `produto`: Métodos de Criação de Produtos
- `marketing`: A Estrutura Narrativa de 3 Atos, Métodos de Escrita de Conteúdo, Métodos Narrativos, Repos (`setor: copywriting`)

**`tipo` por estrutura:**
- Raiz do livro (`X - Autor.md`) → `livro` · `Capítulos/Seções/Partes/Segredos` → `capítulo` · `Repos/*/Exemplos/` → `exemplo` · demais `Repos/` → `fonte` · hubs/sumários → `MOC` · nomes com "Guia" → `guia`

### 6.6 Limpeza de `tags`
1. Remover autores (movidos para `author`).
2. Remover chaves vazadas (`author:`, `dominio:`, `Subdominio:`, `tipo:`, `up:`, `same:`, `down:`, `source:`, `description:`).
3. Remover hashtags textuais (`#funding`, `#case`).
4. Padronizar acentos e minúsculas.
5. Manter como classificação temática livre (ex.: `persuasão`, `storytelling`, `realestate`, `funding`, `luxo`).

---

## 7. Nota de taxonomia = manual do sistema RAG

Criar `3. Recursos/Estudos/Taxonomia de Metadata.md`:
- **Organograma de agentes** (seção 3) com escopos e cadeia de comando — fonte única para o RAG.
- Definição e regras de cada campo.
- Vocabulários controlados.
- **Templates de roteamento** (seção 4).
- Tabela de mapeamento antigo → novo.
- Checklist de frontmatter para arquivos futuros.

---

## 8. Execução e verificação

1. **Script PowerShell** lê os 515 frontmatters (UTF-8 explícito), calcula `tipo`, `nivel`, `agente`, `departamento`, `setor`, `tags` limpas pelas regras da seção 6 e reescreve só o frontmatter — corpo intacto.
2. **Revisão:** `git diff --stat` + amostragem por `tipo` (livro, capítulo, MOC, fonte, exemplo) e conferência de `agente`.
3. Criar a nota de taxonomia (organograma + roteamento).
4. Sem commit (a menos que solicitado).

## 9. Escopo opcional (Fase 2)
Popular Breadcrumbs `up`/`down` em massa (navegação livro→capítulo). Não confundir com cadeia de comando — esta vive só no organograma.
