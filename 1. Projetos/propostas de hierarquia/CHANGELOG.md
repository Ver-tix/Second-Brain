
# Relatório — Passo 7 · Replicar o ciclo em Marketing

> Escopo aprovado: ciclo completo (HUB com autoridade → fontes com tier/status → síntese) replicado em **Marketing**. **Nenhum commit.**

## Decisões do usuário

1. **Departamento:** Marketing (próximo na ordem da proposta).
2. **`autoridade_padrao` do Marketing HUB ("evidência primeiro"):** Sharp/Romaniuk → Kotler/Keller → Ries/Trout → Brunson → Miller → Weinberg/Mares.
3. **Domínios intercambiáveis:** Kotler/Keller (Administração em Marketing) **mantém `dominio: branding`** (Passo 5) — os domínios são intercambiáveis e podem se comunicar; o nome Kotler entra na autoridade do Marketing HUB mesmo assim.
4. **Status das fontes de Marketing:** só as **divergentes de Sharp** ficam `contestado` (Kotler, Ries/Trout, Brunson); Miller e Weinberg/Mares ficam `consenso`.
5. **Síntese:** conflito **Funil vs. Saliência** (Brunson vs Sharp), criada em `1. Projetos` (padrão do Passo 6).

## O que foi alterado (7 arquivos)

### Marketing HUB
- `2. Áreas\2.1. Marketing HUB.md` → adicionado `autoridade_padrao:` (6 autoridades, ordem de desempate).

### 6 fontes de Marketing → `tipo: fonte`, `dominio: marketing`, `subdominio: marketing-estrategico`, `sub_subdominio: ""`, `tier: 1`

| Fonte | Autor | status |
|---|---|---|
| `...\DotCom Secrets\DotCom Secrets - Russell Brunson.md` | Russell Brunson | `contestado` |
| `...\Expert Secrets\Expert Secrets.md` | Russell Brunson | `contestado` |
| `...\Posicionamento\Posicionamento - Al Ries & Jack Trout.md` | Ries & Trout | `contestado` |
| `...\Storybrand\Storybrand - Donald Miller.md` | Donald Miller | `consenso` |
| `...\Tração\Tração - Gabriel Weinberg e Justin Mares.md` | Weinberg & Mares | `consenso` |
| `...\Administração em Marketing\... - Kotler e Keller.md` | Kotler & Keller | `contestado` (mantido do Passo 5, `dominio: branding`) |

- **Correção de formato legado:** `DotCom Secrets` e `Expert Secrets` tinham `tipo:\n  - MOC` (livro marcado como MOC) → corrigidos para `tipo: fonte`. `Posicionamento` tinha `tipo:\n  - fonte`/`dominio:\n  - branding` em lista → normalizado para escalar. Corpos intactos.

### Nova síntese (1 arquivo)
- `1. Projetos\sintese_marketing_funil_vs_saliencia.md` → `tipo: sintese`, `dominio: marketing`, `tier: 0`, `status: sua_posicao_atual`, `resolve_conflito_entre` (Sharp/Romaniuk, Brunson).

## Conteúdo da síntese (posição resolvida)

1. **Saliência é o motor de crescimento; o funil é a ferramenta de conversão** — funil converte demanda que já existe; não gera demanda sem disponibilidade mental.
2. **Funil não é segmentação de nicho** — escada de valor é mecanismo de transação; amplitude de mercado (Sharp) segue como meta.
3. **Onde o funil manda:** ordem de oferta, mensagem e script de conversão (aí entram Miller e Weinberg/Mares como execução).
4. **Regra prática:** marca nova cresce com saliência + teste de canais (Tração); funil entra depois.

## Não executado (fora do escopo)

- `author:` → `autor:` mantido (decisão do Passo 5).
- `up`/`down`/`same` (Breadcrumbs) e `agente`/`setor` (seção 14) — Passos 9+.
- **`Estratégia do Oceano Azul`** (Kim & Mauborgne) e **`O Plano de Marketing de 1 Página`** (Allan Dib): têm tags `business`/`marketing` mas **não entraram** na autoridade nem no status (não listadas pelo usuário). Ficam pendentes de decisão.
- **`Monetizing Innovation`** (Ramanujam/Tacke): também não incluída — fora da lista de autoridade.

## Estado do checklist

- [x] Passo 7 — ciclo completo replicado em Marketing (HUB autoridade → fontes tier/status → síntese Funil vs. Saliência)
- [ ] Passo 8 — arquivos órfãos preenchidos

## Alterações nesta etapa

| Arquivo | Mudança |
|---|---|
| `2. Áreas\2.1. Marketing HUB.md` | + `autoridade_padrao` (6 autoridades) |
| 5 fontes de Marketing | `tipo/dominio/subdominio/tier/status` + correção de `tipo: MOC` legado |
| `1. Projetos\sintese_marketing_funil_vs_saliencia.md` | **novo** — síntese Funil vs. Saliência |
| `1. Projetos\propostas de hierarquia\CHANGELOG.md` | Relatório do Passo 7 |

**Nenhum commit.**


# Relatório — Passo 6 · Primeira nota `sintese`

> Escopo: nota criada em `1. Projetos\texto_opencode.md` (local e nome definidos pelo usuário). Conflito: **posicionamento vs. saliência** — o exemplo citado na proposta (seção 9/Passo 6). **Nenhum commit.**

## O que foi criado

- `1. Projetos\texto_opencode.md` → `tipo: sintese`, `dominio: branding`, `subdominio: branding-estrategico`, `tier: 0`, `status: sua_posicao_atual`, `resolve_conflito_entre:` (Sharp/Romaniuk, Aaker, Kotler/Keller, Holt, Neumeier).

## Conteúdo (posição resolvida)

1. **Saliência = meta primária** — investimento em disponibilidade mental (CEPs) e ativos distintivos; diferenciação percebida é fraca e não explica crescimento (dados de How Brands Grow).
2. **Posicionamento = rebaixado a ferramenta contextual** — orquestração interna/trueline, naming/arquitetura, diferenças funcionais reais; mas depende da saliência para funcionar.
3. **Holt = caso à parte** — cultural branding responde a outra pergunta (ícones/mitos), não concorre com saliência nem posicionamento.
4. **Regra prática** — maduras/crescimento → saliência; lançamentos/naming → posicionamento como guia interno + virar ativo distintivo; ambiente ruidoso → distinctiveness; ambição cultural (Holt) → camada opcional.

> Direção da síntese alinhada à `autoridade_padrao` do HUB (Sharp/Romaniuk #1).

## Estado do checklist

- [x] Passo 6 — primeira síntese escrita (posicionamento vs. saliência)
- [ ] Passo 7 — replicado em outro departamento

## Alterações nesta etapa

| Arquivo | Mudança |
|---|---|
| `1. Projetos\texto_opencode.md` | **novo** — primeira nota `sintese` |
| `1. Projetos\propostas de hierarquia\CHANGELOG.md` | Relatório do Passo 6 |

**Nenhum commit.**

## `git diff --stat` + `git status` (sem commit)

```
 1. Projetos/propostas de hierarquia/CHANGELOG.md  | 283 +++++++++++++++++++++
 1. Projetos/propostas de hierarquia/proposta...md |  34 +--
 "2. Áreas/1. Business HUB.md"                     |  12 +-
 "2. Áreas/2.1. Marketing HUB.md"                  |   6 +-
 "2. Áreas/2.2. Branding HUB.md"                   |  15 +-
 "2. Áreas/3. Inteligência Artificial HUB.md"      |   6 +-
 "2. Áreas/4. Mercado Imobiliário HUB.md"          |   1 +
 "3. Recursos/Estudos/Livros/* (9 fontes Branding) |  7-8 + cada
 16 files changed, 386 insertions(+), 35 deletions(-)
 ?? "1. Projetos/texto_opencode.md"   ← arquivo novo (não aparece no --stat)
```


# Relatório — Passo 5 · Marcar tier e status nas fontes existentes (Branding)

> Escopo aprovado: 9 fontes de Branding com campos da seção 4.2 + `autoridade_padrao` do HUB reordenado. **Nenhum commit.**

## Decisões do usuário

1. **`autoridade_padrao` do Branding HUB** — ordem atualizada para: **Sharp/Romaniuk → Neumeier → Holt → Aaker/Kotler** (era Sharp/Romaniuk → Holt → Aaker/Kotler → Neumeier). Essa ordem resolve contestações ("em caso de contestação, Sharp/Romaniuk > Neumeier > Holt > Aaker/Kotler").
2. **Campo `author:` mantido** — não normalizado para `autor:` (schema 4.2 fala em `autor`, mas a prática da vault é `author`; mantido como está para evitar ruptura em massa).

## O que foi alterado (10 arquivos)

### Branding HUB
- `2. Áreas\2.2. Branding HUB.md` → `autoridade_padrao` reordenado.

### 9 fontes de Branding → `tipo: fonte`, `dominio: branding`, `subdominio: branding-estrategico`, `sub_subdominio: ""`, `tier: 1`, `status: contestado`

| Fonte | Autor | Obra |
|---|---|---|
| `3. Recursos\Estudos\Livros\How Brands Grow\How Brands Grow.md` | Byron Sharp | How Brands Grow |
| `...\How Brands Grow 2\How Brands Grow, parte 2.md` | Sharp & Romaniuk | How Brands Grow 2 |
| `...\Building Distinctive Assets\Building Distinctive Assets.md` | Jenni Romaniuk | Building Distinctive Assets |
| `...\How Brands Become Icons\... - Douglas B. Holt.md` | Douglas B. Holt | How Brands Become Icons |
| `...\Building Strong Brands\... - David A. Aaker.md` | David A. Aaker | Building Strong Brands |
| `...\Managing Brand Equity\... - David A. Aaker.md` | David A. Aaker | Managing Brand Equity |
| `...\Administração em Marketing\... - Kotler e Keller.md` | Kotler & Keller | Administração em Marketing |
| `...\ZAG\ZAG - Marty Neumeier.md` | Marty Neumeier | ZAG |
| `...\THE BRAND GAP\THE BRAND GAP.md` | Marty Neumeier | The Brand Gap |

- `tags` de todas as fontes **intactas** (ex: `branding`, `estratégia`, `tático`, `marketing`).
- `status: contestado` em todas — divergência saliência (Sharp) vs posicionamento/lealdade (Aaker/Kotler) vs cultural branding (Holt) documentada na seção 6.3 da proposta; Neumeier incluído como escola própria. Resolução via `autoridade_padrao` do HUB.
- Corpo das notas intacto.

## Não executado (decisões registradas)

- `author:` → `autor:` **não** convertido (decisão do usuário).
- Campos `up`/`down`/`same` (Breadcrumbs) não preenchidos — fora do escopo do Passo 5 (é o Passo 9/Fase 2).
- Campo `agente`/`setor` (seção 14) não adicionado — Passo 9.

## Estado do checklist

- [x] Passo 5 — fontes de um departamento (Branding) com tier/status
- [ ] Passo 6 — primeira síntese escrita

---

## Alterações nesta etapa

| Arquivo | Mudança |
|---|---|
| `2. Áreas\2.2. Branding HUB.md` | `autoridade_padrao` reordenado (Sharp/Romaniuk → Neumeier → Holt → Aaker/Kotler) |
| 9 notas `fonte` de Branding | `tipo/dominio/subdominio/sub_subdominio/tier/status` |
| `1. Projetos\propostas de hierarquia\CHANGELOG.md` | Relatório do Passo 5 |

**Nenhum commit.**


# Relatório — Passo 4 · Ativar os campos nos HUBs existentes

> Escopo aprovado: Passo 4 + decisões (Mercado Imobiliário sem `dominio`; Vendas/Finanças não criados agora). **Nenhum commit.**

## O que foi alterado (4 arquivos)

| Arquivo                                      | Antes                                    | Depois                                                                                                 |
| -------------------------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `2. Áreas\1. Business HUB.md`                | `dominio:\n  - business e administração` | `dominio: business` + `departamentos:` (marketing, branding, inteligenciaartificial, vendas, financas) |
| `2. Áreas\2.1. Marketing HUB.md`             | `dominio:\n  - marketing`                | `dominio: marketing`                                                                                   |
| `2. Áreas\2.2. Branding HUB.md`              | `dominio:\n  - branding`                 | `dominio: branding` + `autoridade_padrao:` (Sharp/Romaniuk → Neumeier → Holt → Aaker/Kotler)           |
| `2. Áreas\3. Inteligência Artificial HUB.md` | `dominio:\n  - IA`                       | `dominio: inteligenciaartificial`                                                                      |

- `tipo: moc` já estava presente em todos (Passo 2) — mantido.
- `tags` de todos os HUBs **intactas** (ex: `IA`, `programação`, `inovação` no IA HUB).
- Corpo dos arquivos byte a byte intacto.
- Formato `dominio` padronizado para escalar (`dominio: <slug>`), conforme templates da seção 8.

## Decisões registradas

1. **`4. Mercado Imobiliário HUB`** — ficou **sem `dominio`** (só `tipo: moc`), pois não pertence aos 6 domínios aprovados. Pendência: decidir futuro tratamento (mapear como `business`? domínio próprio?) numa etapa futura.
2. **`Vendas HUB` e `Finanças HUB`** — **não existem**; não criados. Pendência registrada para criação quando desejado (frontmatter padrão: `tipo: moc`, `dominio: vendas|financas`).

## Não executado (fora do escopo literal do Passo 4)

- `descricao` no Business HUB (aparece no exemplo da seção 10, mas o Passo 4 não o lista) — não adicionado.
- `autoridade_padrao` em Marketing/IA — sem ordem definida na proposta ("quando fizer sentido"); só Branding tem lista aprovada.

## Estado do checklist

- [x] Passo 4 — HUBs existentes com campos novos (Branding serviu de teste do formato completo, conforme a proposta)

---

## Alterações nesta etapa

| Arquivo | Mudança |
|---|---|
| `2. Áreas\1. Business HUB.md` | `dominio: business` + `departamentos` |
| `2. Áreas\2.1. Marketing HUB.md` | `dominio: marketing` (normalizado) |
| `2. Áreas\2.2. Branding HUB.md` | `dominio: branding` + `autoridade_padrao` |
| `2. Áreas\3. Inteligência Artificial HUB.md` | `dominio: inteligenciaartificial` |
| `1. Projetos\propostas de hierarquia\CHANGELOG.md` | Relatório do Passo 4 |

**Nenhum commit.** Pendências: Mercado Imobiliário (sem dominio) · Vendas/Finanças HUBs (não criados).


# Relatório — Passo 3 · Hierarquia de departamentos

> Escopo aprovado: escrever por extenso a árvore completa (draft) para fixar os slugs exatos. **Nenhum frontmatter de nota foi alterado** — a aplicação dos slugs é o Passo 4. Nenhum commit.

## Árvore completa (draft — seção 3 da proposta, transcrita por extenso)

```
business (CEO) — estrategico
├── proposta-de-valor
├── segmentos-de-cliente
├── canais
├── relacionamento-com-cliente
├── fontes-receita
├── estrutura-custos
├── recursos-chave
├── atividades-chave
├── parcerias-chave
├── inovacao                      # fora dos 9 blocos do BMC — adição própria
├── marketing-estrategico (CMO)
│	├──marketing-tatico-acoes_estruturantes
│   └── marketing-tatico-mix
│       ├── marketing-tatico-preco
│       ├── marketing-tatico-praca
│       ├── marketing-tatico-promocao
│       └── marketing-tatico-produto   # microverso fractal — pode ter agente cpo
├── branding-estrategico (CBO)
│   └── branding-tatico
│       ├── branding-tatico-identidade_verbal-naming
│       ├── branding-tatico-identidade_verbal-tom_de_voz
│       ├── branding-tatico-identidade_verbal-messaging
│       ├── branding-tatico-identidade_verbal-ativos_distintivos_verbais
│       ├── branding-tatico-identidade_visual-logo
│       ├── branding-tatico-identidade_visual-tipografia
│       ├── branding-tatico-identidade_visual-cores
│       ├── branding-tatico-identidade_visual-ativos_distintivos_visuais
│       ├── branding-tatico-personalidade-arquetipos
│       ├── branding-tatico-experiencia_de_marca-cultural_branding
│       ├── branding-tatico-experiencia_de_marca-touchpoints
│       ├── branding-tatico-experiencia_de_marca-atendimento
│       └── branding-tatico-experiencia_de_marca-embalagem
├── inteligenciaartificial (CTO)   # categorias táticas: ainda não definidas
├── vendas (CSO)                   # categorias táticas: ainda não definidas
└── financas (CFO)                 # categorias táticas: ainda não definidas
```

## Regras que valem pra todos os nós

1. `dominio` em toda nota é o slug do departamento: `business` · `marketing` · `branding` · `inteligenciaartificial` · `vendas` · `financas` — **nunca** o nome do nó da árvore (ex: `marketing-estrategico` é `subdominio`, não `dominio`).
2. Os nós da árvore são os slugs de `subdominio` (`dominio-nivel` / `dominio-nivel-categoria`). Rótulos `(CEO)`/`(CMO)` etc. são só títulos de cargo.
3. `nivel` é **relativo ao pai imediato** (`up`), não ao topo — princípio fractal (6.9). Ex: `marketing-tatico-preco` é tático dentro de Marketing, mas a árvore inteira volta a valer dentro do microverso de Produto.
4. Slugs sempre **sem acento** (convenção 6.10): `preco`, `praca`, `estrategico`, `tatico`, `operacional`.

## ✅ Divergências de slug — resolvidas (aprovado)

Ao transcrever a seção 3, detectei 2 pontos onde a árvore quebrava o próprio padrão `dominio-nivel` que usa em Marketing. **Aprovação do usuário recebida — normalização aplicada na `proposta_claude_hierarquiaV3.md` (seção 3) e refletida no draft acima:**

1. **`brand-strategy`** (inglês) → **`branding-estrategico`** (`dominio-nivel`, igual a `marketing-estrategico`).
2. **Folhas de Branding sem o prefixo `branding-`** — `tatico-identidade_verbal-naming` … `tatico-experiencia_de_marca-embalagem` → **`branding-tatico-identidade_verbal-naming`** … **`branding-tatico-experiencia_de_marca-embalagem`** (`dominio-nivel-categoria`, igual a `marketing-tatico-preco`).
3. **`marketing-tatico-mix`** — registrado, sem mudança (nó que agrupa os 4 Ps).

**Obs.:** `Metadata Mapping.md` (na mesma pasta) é uma versão **legada** da proposta (ainda usa `brand-strategy`, folhas sem prefixo e acentos em `preço/praça/promoção`). **Não foi tocado** — se quiser consolidá-lo com a V3, é uma ação separada.

## Estado do checklist

- [x] Passo 3 — árvore de departamentos escrita por extenso (draft acima; divergências de slug **aprovadas e normalizadas** na proposta V3)

---

## Alterações nesta etapa

| Arquivo | Mudança |
|---|---|
| `1. Projetos\propostas de hierarquia\proposta_claude_hierarquiaV3.md` | Seção 3: `brand-strategy` → `branding-estrategico`; folhas de Branding prefixadas com `branding-` |
| `1. Projetos\propostas de hierarquia\CHANGELOG.md` | Relatório do Passo 3 + registro da aprovação |

**Nenhum frontmatter de nota foi alterado.** Nenhum commit.


# Relatório — Passo 2 · Os 4 tipos de nota

> Escopo aprovado: normalizar `tipo` nos 5 HUBs + relatório de diagnóstico. **Nenhum commit realizado.**

## O que foi alterado (5 arquivos)

Os 5 HUBs de domínio passaram a carregar `tipo: moc` no formato da seção 4.1:

| Arquivo | Antes | Depois |
|---|---|---|
| `2. Áreas\1. Business HUB.md` | `tipo:` + `- MOC` | `tipo: moc` |
| `2. Áreas\2.1. Marketing HUB.md` | `tipo:` + `- MOC` | `tipo: moc` |
| `2. Áreas\2.2. Branding HUB.md` | `tipo:` + `- MOC` | `tipo: moc` |
| `2. Áreas\3. Inteligência Artificial HUB.md` | `tipo:` + `- MOC` | `tipo: moc` |
| `2. Áreas\4. Mercado Imobiliário HUB.md` | (sem campo `tipo`) | `tipo: moc` |

- Nenhum outro campo foi alterado (`tags`, `dominio`, etc. **intactos**). Corpo dos arquivos byte a byte intacto.
- O `Mercado Imobiliário HUB` só ganhou o campo `tipo` — era o único HUB sem ele.

---

## Diagnóstico — campo `tipo` no vault inteiro

Escopo analisado: **975 notas .md** (excluídos `.trash`, `.obsidian`, `.git`, `.mimocode` — infraestrutura de ferramenta — e a pasta `propostas de hierarquia`).

**211 notas (21,6%) já têm `tipo`:**

| `tipo` | Qtde |
|---|---|
| `fonte` | 150 |
| `conceito` | 40 |
| `moc` | 20 |
| `exemplo` | 1 ⚠️ |

**764 notas (78,4%) não têm `tipo`:**

| Localização | Qtde |
|---|---|
| `3. Recursos` | 743 |
| `1. Projetos` | 17 |
| `4. Arquivos` | 3 |
| `README.md` (raiz) | 1 |

---

## Achados do diagnóstico

1. **Os 4 tipos aprovados já estão em uso** (`moc`/`fonte`/`conceito`), com forte predominância de `fonte` (150) — coerente com a base de Livros/Métodos. **`sintese` (Tier 0) ainda não existe nenhuma** — é o tipo mais valioso pro RAG e o Passo 6 prevê escrever a primeira.
2. **Outlier `exemplo`** (1 arquivo): `3. Recursos\Estudos\Cursos\Marketing\SEO\SEO, AEO e GEO\Seção 2 - Exemplo de Otimização e Projeto\5. Como Otimizar uma Página para todos - AEO, GEO LLM SEO e SEO.md` usa `tipo: exemplo`, que **não pertence** ao vocabulário (moc/fonte/conceito/sintese). Candidato natural a `fonte`. **Não tocado** — decisão pendente.
3. **Formato de lista maiúsculo `- MOC`**: além dos HUBs (corrigidos aqui), outros MOCs do vault (ex: `SEO MOC`, `Headlines - MOC`, `🔥Projeto Prometheus MOC`, `Dicionário MOC`, `Métodos de Criação de Produtos MOC`, `Textos Completos - MOC`) ainda usam `tipo:\n  - MOC`. Normalização restante fica para etapa futura.
4. **Passo 4 não foi executado**: os HUBs ainda precisam ganhar `dominio`, `departamentos` (Business) e `autoridade_padrao` (Branding, quando fizer sentido) — etapa separada do Passo 2.

---

## `git diff --stat` (sem commit)

```
 1. Projetos/propostas de hierarquia/CHANGELOG.md  | 67 ++++++++++++++++++++++++
 1. Projetos/propostas de hierarquia/proposta...md |  8 +-  ← checklist [x] (usuário)
 "2. Áreas/1. Business HUB.md"                     |  3 +-
 "2. Áreas/2.1. Marketing HUB.md"                  |  3 +-
 "2. Áreas/2.2. Branding HUB.md"                   |  3 +-
 "2. Áreas/3. Inteligência Artificial HUB.md"      |  3 +-
 "2. Áreas/4. Mercado Imobiliário HUB.md"          |  1 +
 7 files changed, 76 insertions(+), 12 deletions(-)
```

> **`proposta_claude_hierarquiaV3.md`**: alteração feita pelo próprio usuário (checklist da seção 12 — Passos 0, 1 e 2 marcados como concluídos). **Não faz parte do Passo 2 e não foi alterada por mim.** Não revertida.


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
