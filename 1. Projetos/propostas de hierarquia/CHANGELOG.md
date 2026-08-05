
# Relatório — Passo 1 · Limpeza básica de tags (ETAPA 2 — aplicação)

> Aplicação concluída. **Nenhum commit realizado.**

## Escopo aprovado × executado

| Ação | Aprovado | Executado |
|---|---|---|
| `#funding` → `funding` | sim | ✓ |
| `#case` → `case` | sim | ✓ |
| Remover `instruction` | sim | ✓ |
| Remover `descoberta` | sim | ✓ |
| Remover `notes` | sim | ✓ |
| Remover `dicionario` | sim | ✓ |
| `IA` → `ia` | **não** | — (intacto) |

---

## 1. O que foi alterado (31 arquivos)

### 1.1 `#` removido — 2 arquivos
- `3. Recursos\Estudos\Livros\Crédito Vale Mais do Que Dinheiro\Crédito Vale Mais do Que Dinheiro.md`
  - `- "#funding"` → `- funding`
- `3. Recursos\Estudos\Livros\Monetizing Innovation\...\Capítulo 1 - Um Conto de Dois Carros.md`
  - `- "#case"` → `- case`

### 1.2 Tag `instruction` removida — 1 arquivo
- `1. Projetos\README.md` (era artefato de prompt; as tags `marketing`, `business`, `IA`, `realestate` foram mantidas)

### 1.3 Tag `descoberta` removida — 13 arquivos
- Todos os capítulos de `3. Recursos\Estudos\Livros\How Brands Grow\` (cap. 1 a 13). Mantida a tag `branding`.

### 1.4 Tag `notes` removida — 8 arquivos (frontmatter inteiro removido, pois só tinha essa tag)
- `3. Recursos\Anotações\Notas\Notas.md`
- `3. Recursos\Anotações\Notas\Diário de Bordo\2026-07-09.md`
- `3. Recursos\Anotações\Notas\Diário de Bordo\2026-07-10.md`
- `3. Recursos\Anotações\Notas\Diário de Bordo\2026-07-11.md`
- `3. Recursos\Anotações\Notas\Diário de Bordo\2026-07-12.md`
- `3. Recursos\Anotações\Notas\Diário de Bordo\2026-07-14 - 2026-07-18.md`
- `3. Recursos\Anotações\Notas\Projetos\Projeto Second Brain Omnipresent\Estruturar Agentes de IA.md`
- `3. Recursos\Anotações\Notas\Projetos\Projeto Second Brain Omnipresent\LEIA. IMPORTANTE PARA EVITAR DANOS AO PROJETO.md`

### 1.5 Tag `dicionario` removida — 7 arquivos
- `3. Recursos\Estudos\Dicionário\Dicionário.md`
- `3. Recursos\Estudos\Dicionário\Economia de Escala.md`
- `3. Recursos\Estudos\Dicionário\Lojas Pop-Up.md`
- `3. Recursos\Estudos\Dicionário\Lojas-Conceito.md`
- `3. Recursos\Estudos\Dicionário\Planograma (Plan-o-Gram).md`
- `3. Recursos\Estudos\Dicionário\Unidades de Negócio.md`
- `3. Recursos\Estudos\Dicionário\Vendedor Silencioso.md`

---

## 2. Garantias de integridade

- **Somente frontmatter** foi tocado; o corpo de todos os 31 arquivos está **byte a byte intacto** (verificado via `git diff`).
- **Nenhum outro campo** foi alterado (`tipo`, `dominio`, `Subdominio`, `author`, `up`, etc.).
- **`IA` não foi tocada** — 287 arquivos permanecem com a tag `IA` (decisão do usuário).
- Encoding UTF-8 e line endings preservados.

---

## 3. Estado final das tags

- 45 tags distintas antes → **39 tags distintas agora**
- Removidas: `instruction`, `descoberta`, `notes`, `dicionario`
- `case`: 68 → **69** (recebeu o `#case`) · `funding`: 8 → **9** (recebeu o `#funding`)
- Nenhuma tag com `#` restante; nenhuma tag com chave vazada; nenhum autor em `tags`

---

## 4. `git diff --stat` (sem commit)

```
 1. Projetos/README.md                                                | 1 -
 2. Recursos/Anotações/Notas/* (8 arquivos)                           | 4-5 linhas removidas cada
 3. Recursos/Estudos/Dicionário/* (7 arquivos)                        | 1 - cada
 4. Recursos/Estudos/Livros/How Brands Grow/* (13 arquivos)           | 1 - cada
 5. Recursos/Estudos/Livros/Crédito.../Crédito Vale Mais...md         | 2 +-
 6. Recursos/Estudos/Livros/Monetizing Innovation/.../Capítulo 1...md | 2 +-
 README.md (raiz)                                                     | 4 ----  ← NÃO é do Passo 1
 32 files changed, 2 insertions(+), 62 deletions(-)
```

---

## 5. Nota sobre arquivos fora deste relatório

- **`README.md` (raiz)** e **`1. Projetos\propostas de hierarquia\CHANGELOG.md`**: alterações feitas pelo próprio usuário (16:37 e 16:45). **Não fazem parte do Passo 1 e não foram alteradas por mim.** Não revertidas.

Nada foi commitado nem enviado (`git status` permanece com as mudanças acima pendentes).


---
# Relatório — Passo 1 · Limpeza básica de tags (ETAPA 1 — dry-run)

> Nenhum arquivo foi alterado. Análise de leitura do estado atual do vault.

## Contexto verificado

- Branch atual: **`reforma-metadados`** ✓ (única branch com trabalho pendente; `main` idêntica)
- Escopo analisado: todos os **971 arquivos .md com frontmatter `tags`** do vault (excluídos `.trash`, `.obsidian`, `.git` e a pasta `propostas de hierarquia`)
- **Achado importante:** o vault já está **~95% limpo**. Grande parte do Passo 1 descrito na proposta (`proposta_claude_hierarquiaV3.md`, seção 11) **não corresponde mais à realidade atual** — provavelmente a proposta foi escrita sobre uma auditoria anterior e parte da limpeza já foi feita manualmente.

---

## 1. Contagem de arquivos por tipo de problema (estado real)

| # | Problema | Proposta dizia | Estado real | Arquivos |
|---|---|---|---|---|
| 1 | `#` dentro de `tags` | 6 arquivos (`#estratégia`, `#Funding`, `#descoberta`, `#dicionario`, `#inteligenciaartificial`, `#tecnologia`) | **2 arquivos** (`#funding`, `#case`) | 2 |
| 2 | Typo `brandig` → `branding` | sim | **0** (já limpo) | 0 |
| 3 | Singular/plural (`case`/`cases`, `funil`/`funis`, `tático`/`táticas`) | sim | **0 pares** — só existe forma única de cada (`case` 68, `funil` 11, `tático` 87) | 0 |
| 4 | Capitalização (`BrandEquity`, `RealEstate`, `Funding`, `Prompts`, `RAG`) | sim | **Só resta `IA`** (287). Todos os outros já estão lowercase (`brandequity` 12, `realestate` 15, `funding` 8, `prompts` 2, `rag` 2) | 287 |
| 5 | Chaves vazadas (`author:`, `dominio:`, `Subdominio:`, `tipo:`, `up:`…) dentro de `tags` | sim | **0** (não há nenhuma linha com `:` nas tags) | 0 |
| 6 | Nomes de autores dentro de `tags` | sim | **0** (nenhuma das 45 tags é nome de autor) | 0 |
| 7 | Tags órfãs/ambíguas | `brandDesign`, `comportamental`, `instruction`, `luxo`, `segmentação`, `vendas` | `brandDesign` **não existe mais**; as outras 5 existem + 3 raras novas | ver seção 3 |

**Vocabulário completo atual: 45 tags distintas, 2.018 ocorrências em 971 arquivos.**

---

## 2. Correções mecânicas claras (sem ambiguidade)

| Tag atual | Forma final | Arquivos | Justificativa |
|---|---|---|---|
| `#funding` → `funding` | remover `#` | 1 | `funding` já é tag existente (8 arquivos); o livro é sobre financiamento/crédito imobiliário |
| `#case` → `case` | remover `#` | 1 | `case` já é tag existente (68 arquivos); arquivo é estudo de caso |
| `IA` → `ia` | lowercase | **287** | Regra "lowercase geral" da proposta (11.4); `RAG`→`rag` já foi feito no vault, `IA` é o único maiúsculo restante. **⚠️ Volume grande — precisa de aprovação explícita** |

---

## 3. Tags órfãs/ambíguas — forma final escolhida e por quê

### Parecem ERRO → recomendo remover

| Tag | Arquivos | Por quê |
|---|---|---|
| `instruction` | 1 (`1. Projetos\README.md`) | Palavra inglesa genérica, sem relação temática; artefato típico de prompt de sistema colado no README. Não é classificação. |
| `descoberta` | 13 (todos os capítulos de How Brands Grow) | Aplicada em massa a **todos** os capítulos do livro (1 a 13) — padrão de template legado (a proposta citava `#descoberta` entre os hashtags). Sem poder discriminante. |
| `notes` | 8 (Diário de Bordo + Notas.md) | Tag genérica em inglês de template do Obsidian (daily notes), não temática. Baixo valor. |
| `dicionario` | 7 (todos na pasta Dicionário) | Redundante com a pasta; não discrimina. |

### São classificação temática LEGÍTIMA → manter como estão

| Tag | Arquivos | Por quê |
|---|---|---|
| `comportamental` | 1 (Logica do Consumo - Martin Lindstrom) | Tema real: comportamento do consumidor (Buyology). |
| `luxo` | 1 (Produtos de Luxo) | Tema real de marketing. |
| `segmentação` | 2 (O Plano de Marketing de 1 Página; Monetizing Innovation) | Notas sobre segmentação/público-alvo; coincide com o `setor` do organograma (14.5). |
| `vendas` | 1 (DotCom Secrets - Introdução Seção 1) | Tema vendas; reforça o domínio `vendas` do organograma (14.2). |
| `unificador` | 1 (artigo "(UNIFICADOR)" do Prometheus) | Marcador interno intencional do curso. |
| `shortstaying` | 2 (AirBNB) | Nicho real: aluguel de curta temporada. |
| `e-mail` | 1 (E-mail Marketing) | Tema real. *Obs.: o vocabulário de `setor` é `email-marketing` — alinhar fica para a etapa de `setor`, não no Passo 1.* |
| `comercial` | 2 (scripts de fechamento/conversão) | Tema vendas/conversão, consistente nos 2 usos. |
| `economia` (2), `tecnologia` (2), `design` (3), `serviços` (7), `artigo` (5), `projeto` (5), `clippings` (5), `headline` (9), `naming` (5), `copywriting` (9), `posicionamento` (15), `funil` (11), `case` (68), `prompts` (2), `rag` (2), `canais` (93), `produto` (36), `precificação` (19), `promoção` (51), `estratégia` (77), `tático` (87), `operacional` (87), `programação` (147), `inovação` (106), `marketing` (476), `business` (112), `branding` (213), `brandequity` (12), `realestate` (15) | — | Temas consistentes, em lowercase (exceto `IA`), com acento liberado em tags (convenção 6.10). |

---

## 4. Exemplos concretos antes/depois

**A. Remoção de `#`:**
```
3. Recursos\Estudos\Livros\Crédito Vale Mais do Que Dinheiro\Crédito Vale Mais do Que Dinheiro.md
   - realestate            →   - realestate
   - "#funding"            →   - funding

3. Recursos\Estudos\Livros\Monetizing Innovation\...\Capítulo 1 - Um Conto de Dois Carros.md
   - marketing             →   - marketing
   - "#case"               →   - case
```

**B. Capitalização:**
```
1. Projetos\framework_agentes_arquitetura.md
   - IA                   →   - ia
   - programação          →   - programação
   (o campo dominio: IA NÃO será tocado — fora do escopo)
```

**C. Remoção de órfã suspeita:**
```
1. Projetos\README.md
   - instruction    →   (removida)
   - marketing      →   - marketing

3. Recursos\Estudos\Livros\How Brands Grow\1 - Marketing Baseado em Evidências.md
   - branding       →   - branding
   - descoberta     →   (removida, se aprovado)
```

---

## 5. Proposta de escopo da ETAPA 2 (aguardando aprovação)

1. **Remover `#`** → 2 arquivos (`#funding`→`funding`, `#case`→`case`)
2. **`IA` → `ia`** → 287 arquivos
3. **Remover `instruction`** → 1 arquivo (recomendado)
4. **Remover `descoberta`** → 13 arquivos (recomendado — manter se preferir)
5. **`notes` e `dicionario`** → remoção opcional, só com confirmação

Sem nenhum commit. Aprovação explícita (e decisão sobre os itens 2–5) antes de alterar qualquer arquivo.
