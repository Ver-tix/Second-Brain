# Proposta: Sistema de Metadata Departamental — Livros & Métodos

**Autor:** opencode · **Data:** 05/08/2026 · **Escopo:** `3. Recursos/Estudos/Livros` (415 arquivos) e `3. Recursos/Estudos/Métodos` (100 arquivos) · **Total:** 515 arquivos

---

## 1. Contexto atual

Todos os 515 arquivos possuem frontmatter. O modelo atual usa os seguintes campos:

| Campo | Ocorrências | Observação |
|---|---|---|
| `tags` | 513 | Com autores, chaves de metadata vazadas e acentos inconsistentes |
| `author` | 216 | Correto, mantido |
| `dominio` | 153 | `marketing`, `branding`, `business e administração` |
| `Subdominio` | 148 | Mistura departamento + nível (ex.: `marketing-tático-mix`) |
| `tipo` | 136 | Apenas `MOC` e `fonte` |
| `Sub_subdominio` | 133 | Quase equivalente a "setor" |
| `description` | 15 | Mantido como campo livre |
| `up` / `same` / `down` | 11 / 9 / 1 | Campos do plugin Breadcrumbs |
| `source` | 3 | URLs, mantido |

### Problemas identificados

1. **Autores dentro de `tags`** — `Russell Brunson`, `Gabriel Weinberg`, `Robert W. Bly`, `Philip Kotler`, etc. aparecem como tags, mas deveriam ficar apenas em `author`.
2. **Chaves de metadata vazadas em `tags`** — `author:`, `dominio:`, `Subdominio:`, `tipo:`, `up:`, `same:`, `down:` aparecem como itens de `tags`.
3. **`tipo` insuficiente** — não distingue `livro`, `capítulo`, `método`, `exemplo`, `case`, `guia`.
4. **Acentos/case inconsistentes** — `marketing-estrategico` vs `marketing-tático` vs `marketing-tatico-mix`; `analise-mercados`.
5. **~360 arquivos sem hierarquia** — só possuem `tags` + `author`.
6. **Nomeação confusa** — `Subdominio` / `Sub_subdominio` misturam eixo de nível e eixo departamental.

---

## 2. Novo esquema YAML

A organização dos metadados segue a **departamentalização de empresas**: CEO no topo estratégico, departamentos C-Level no nível tático e setores no nível operacional.

```yaml
---
tipo: capítulo        # livro | capítulo | método | MOC | fonte | exemplo | case | guia
nivel: tático         # estratégico | tático | operacional
departamento: marketing  # business | marketing | branding | produto | vendas | finanças
setor: copywriting    # copywriting | precificação | promoção | praça | funil-de-vendas | ...
tags:
  - copywriting
author:
  - Robert W. Bly
source: https://...   # mantém quando existir
description: "..."    # mantém quando existir
up: "[[...]]"         # campos Breadcrumbs preservados
same: [...]
down: [...]
---
```

### Modelo conceitual — dois eixos

- **Eixo hierárquico (`nivel`):** altura na pirâmide organizacional.
  - `estratégico` → CEO / C-Suite, decisões de longo prazo.
  - `tático` → departamentos C-Level (CMO, CBO…), planos de médio prazo.
  - `operacional` → setores, execução cotidiana.
- **Eixo departamental (`departamento`):** quem é "dono" do conteúdo na organização.

**Exemplos de classificação:**
- `How Brands Grow` → `nivel: estratégico` + `departamento: marketing` (estratégia de marca sob o CMO).
- `Competitive Advantage` → `nivel: estratégico` + `departamento: business` (estratégia corporativa do CEO).
- `Como Escrever Uma Copy Que Vende` → `nivel: operacional` + `departamento: marketing` + `setor: copywriting`.
- `Balanced Scorecard` → `nivel: estratégico` + `departamento: business` + `setor: estratégia`.

---

## 3. Vocabulários controlados

### `tipo`
| Valor | Uso |
|---|---|
| `livro` | Ficha principal do livro (arquivo na raiz da pasta) |
| `capítulo` | Capítulos, seções, partes e segredos do livro |
| `método` | Frameworks e metodologias de `Métodos/` |
| `MOC` | Map of Content / hubs de sumário |
| `fonte` | Swipe files e materiais de referência (`Repos/`) |
| `exemplo` | Exemplos prontos (`Repos/*/Exemplos/`) |
| `case` | Estudos de caso |
| `guia` | Guias passo a passo |

### `nivel`
`estratégico` · `tático` · `operacional`

### `departamento`
| Valor | C-Level correspondente |
|---|---|
| `business` | CEO / C-Suite (estratégia corporativa) |
| `marketing` | CMO |
| `branding` | CBO |
| `produto` | CPO |
| `vendas` | CSO |
| `finanças` | CFO |

> Outros departamentos podem emergir conforme o vocabulário evolui; registrá-los na nota de taxonomia.

### `setor`
`copywriting` · `precificação` · `promoção` · `praça` · `produto` · `funil-de-vendas` · `segmentação` · `estratégia` · `posicionamento` · `naming` · `storytelling` · `e-mail-marketing` · `marketing-de-conteúdo` · `SEO` · `canais` · `marca` · `inovação` · `valores`

---

## 4. Regras de migração

### 4.1 Mapeamento `dominio` → `departamento`
| Atual | Novo |
|---|---|
| `marketing` | `departamento: marketing` |
| `branding` | `departamento: branding` |
| `business e administração` | `departamento: business` |

### 4.2 Mapeamento `Subdominio` → `nivel` + `setor`
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

### 4.3 Mapeamento `Sub_subdominio` → `setor`
| Atual | Novo |
|---|---|
| `copywriting` | `copywriting` |
| `precificação` | `precificação` |
| `promoção` | `promoção` |
| `praça` | `praça` |
| `produto` | `produto` |
| `segmentação-cliente` / `segmentação-mercado` | `segmentação` |
| `funil-de-vendas` | `funil-de-vendas` |
| `planejamento-estratégico` / `análise-estratégica` / `diagnóstico-estratégico` | `estratégia` |
| `ações-estruturantes` | `estratégia` |
| `slogan` | `naming` |
| `valores` | `valores` |
| `analise-mercados` | `estratégia` |
| `e-mail_marketing` | `e-mail-marketing` |

### 4.4 Classificação dos ~360 arquivos sem hierarquia
Inferida por **pasta + nome do arquivo**:

**Livro → departamento (24 livros):**
- `marketing`: Administração em Marketing, Como Escrever Uma Copy Que Vende, DotCom Secrets, Expert Secrets, Manual do Copywriter, Marketing Canvas, O Plano de Marketing de 1 Página, Storybrand, Tração
- `branding`: Building Distinctive Assets, Building Strong Brands, How Brands Become Icons, How Brands Grow, How Brands Grow 2, Logica do Consumo, Managing Brand Equity, O Herói e o Fora-da-Lei, O Poder do Naming, Posicionamento, THE BRAND GAP, ZAG
- `business`: Competitive Advantage, Estratégia do Oceano Azul, Monetizing Innovation, Crédito Vale Mais do Que Dinheiro

**Método → departamento (10 métodos):**
- `business`: 10 Tipos de Inovação, Balanced Scorecard, Business Model Canvas, V.R.I.O Framework, Value Proposition Design
- `produto`: Métodos de Criação de Produtos
- `marketing`: A Estrutura Narrativa de 3 Atos, Métodos de Escrita de Conteúdo, Métodos Narrativos, Repos (Copywriting → `setor: copywriting`)

**`tipo` por estrutura de arquivo:**
- Raiz da pasta do livro (`X - Autor.md`) → `tipo: livro`
- `Capítulos/`, `Seções/`, `Partes/`, `Segredos/` → `tipo: capítulo`
- `Repos/*/Exemplos/` → `tipo: exemplo`
- Demais arquivos de `Repos/` → `tipo: fonte`
- Hubs/sumários → `tipo: MOC` (mantém)
- Nomes com "Guia" → `tipo: guia`

### 4.5 Limpeza de `tags`
1. Remover nomes de autores (movidos para `author`).
2. Remover chaves vazadas (`author:`, `dominio:`, `Subdominio:`, `tipo:`, `up:`, `same:`, `down:`, `source:`, `description:`).
3. Remover hashtags textuais (`#funding`, `#case`).
4. Padronizar acentos (`marketing-tatico-mix` → `marketing-tático-mix`) e minúsculas.
5. Manter `tags` como classificação temática livre (ex.: `persuasão`, `storytelling`, `realestate`, `funding`, `luxo`, `manifesto`).

---

## 5. Nota de taxonomia (documentação central)

Criar `3. Recursos/Estudos/Taxonomia de Metadata.md` contendo:
- Organograma CEO → CMO/CBO → setores.
- Definição e regras de cada campo.
- Vocabulários controlados (tabelas acima).
- Tabela de mapeamento antigo → novo.
- Regras para arquivos futuros (checklist de frontmatter).

---

## 6. Execução e verificação

1. **Script PowerShell** que lê os 515 frontmatters (UTF-8 explícito, preservando acentos), calcula o novo bloco pelas regras da seção 4 e reescreve apenas o frontmatter — o corpo do arquivo fica intacto.
2. **Revisão:** `git diff --stat` + amostragem de arquivos de cada `tipo` (livro, capítulo, MOC, fonte, exemplo).
3. Criar a nota de taxonomia.
4. Sem commit (a menos que solicitado).

## 7. Escopo opcional (Fase 2)
Popular os campos do plugin **Breadcrumbs** (`up`/`down`) em massa — ex.: todo `capítulo` aponta para o livro pai. Incluído apenas se desejado.
