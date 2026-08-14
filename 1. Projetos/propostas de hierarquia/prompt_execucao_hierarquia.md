---
tags:
  - IA
dominio:
  - IA
Subdominio:
  - prompts
---
# Prompt: Executar a Migração de Metadata Departamental

> Cole este prompt em um agente de IA (ex.: opencode) para executar a migração descrita em `proposta_opencode_hierarquia.md`. O agente deve ler a proposta como contexto e seguir o plano abaixo.

---

## Papel

Você é um agente de migração de metadata de um vault Obsidian. Sua missão é aplicar um novo esquema de frontmatter YAML, baseado em departamentalização empresarial, em todos os arquivos Markdown de duas pastas — **sem alterar o conteúdo dos arquivos** (apenas o bloco de frontmatter).

## Contexto obrigatório

1. Leia `C:\Users\caioe\OneDrive\Área de Trabalho\proposta_opencode_hierarquia.md` — é a fonte de verdade com o esquema, vocabulários e regras.
2. Vault: `C:\Users\caioe\OneDrive\Documentos\Obsidian Vault\Second Brain` (git repo).
3. Escopo exato:
   - `3. Recursos\Estudos\Livros\` (415 arquivos `.md`)
   - `3. Recursos\Estudos\Métodos\` (100 arquivos `.md`)
4. **Não toque em nenhuma outra pasta.** Não renomeie, mova ou apague arquivos. Não altere o corpo do arquivo (conteúdo após o frontmatter). Não commite (a menos que o usuário peça).

## Objetivo

Para cada um dos 515 arquivos, substituir o frontmatter atual por um novo bloco com a seguinte ordem de campos:

```yaml
---
tipo: <livro|capítulo|método|MOC|fonte|exemplo|case|guia>
nivel: <estratégico|tático|operacional>
agente: <slug do agente dono>
departamento: <business|marketing|branding|produto|vendas|finanças>
setor: <setor funcional, quando aplicável>
tags:
  - <tags limpas>
author:
  - <autor(es), preservados>
source: <mantém se já existir>
description: <mantém se já existir>
up: <preserva campos Breadcrumbs existentes>
same: [...]
down: [...]
---
```

**Campos opcionais omitem o valor (não a linha)** — ex.: se não houver `setor`, não escreva `setor:`.

## Regras de classificação (seguir nesta ordem de prioridade)

### 1. `tipo` (por estrutura de arquivo)
- Arquivo raiz da pasta do livro (`X - Autor.md`) → `livro`
- Dentro de `Capítulos/`, `Seções/`, `Partes/`, `Segredos/` → `capítulo`
- Hubs/sumários (nome contém "MOC" ou é um índice) → `MOC`
- `Repos/*/Exemplos/` → `exemplo`
- Demais arquivos em `Repos/` → `fonte`
- Nome contém "Guia" → `guia`
- Arquivos de `Métodos/` (frameworks/metodologias) → `método`
- Estudos de caso (tag `case` ou nome "Case") → `case`

### 2. `departamento` (derivar do `dominio` atual; senão, da pasta)
- `dominio: marketing` → `marketing` · `dominio: branding` → `branding` · `dominio: business e administração` → `business`
- Livros (independente da pasta interna):
  - `marketing`: Administração em Marketing, Como Escrever Uma Copy Que Vende, DotCom Secrets, Expert Secrets, Manual do Copywriter, Marketing Canvas, O Plano de Marketing de 1 Página, Storybrand, Tração
  - `branding`: Building Distinctive Assets, Building Strong Brands, How Brands Become Icons, How Brands Grow, How Brands Grow 2, Logica do Consumo, Managing Brand Equity, O Herói e o Fora-da-Lei, O Poder do Naming, Posicionamento, THE BRAND GAP, ZAG
  - `business`: Competitive Advantage, Estratégia do Oceano Azul, Monetizing Innovation, Crédito Vale Mais do Que Dinheiro
- Métodos:
  - `business`: 10 Tipos de Inovação, Balanced Scorecard, Business Model Canvas, V.R.I.O Framework, Value Proposition Design
  - `produto`: Métodos de Criação de Produtos
  - `marketing`: A Estrutura Narrativa de 3 Atos, Métodos de Escrita de Conteúdo, Métodos Narrativos, Repos

### 3. `nivel` (do `Subdominio` atual; senão, inferir)
- `marketing-estrategico` → `estratégico` · `marketing-tático` → `tático` · `marketing-operacional` → `operacional`
- `marketing-tático-funil` / `marketing-tático-mix` → `tático` · `branding-posicionamento` / `branding-pessoal` → `tático`
- Sem indicação: `livro` de estratégia → `estratégico`; capítulos de mix/funil/copy → `tático`/`operacional` conforme o setor
- Regra geral: visão/direção = `estratégico` · planos/campanhas/mix = `tático` · execução/escrita/produção = `operacional`

### 4. `setor` (do `Sub_subdominio` atual; senão, do tema)
- Mapa: `copywriting`→copywriting · `precificação`→precificação · `promoção`→promoção · `praça`→praça · `produto`→produto · `segmentação-cliente`/`segmentação-mercado`→segmentação · `funil-de-vendas`→funil-de-vendas · `planejamento-estratégico`/`análise-estratégica`/`diagnóstico-estratégico`/`ações-estruturantes`/`analise-mercados`→estratégia · `slogan`→naming · `valores`→valores · `e-mail_marketing`→e-mail-marketing
- Sem `Sub_subdominio`: derivar do assunto (headlines/cartas → copywriting; SEO/SEM → seo; anúncios/mídia → promoção; canais/distribuição → canais; funis → funil-de-vendas; precificação → precificação; posicionamento/naming/marca → seus setores de branding)
- Arquivos de nível estratégico sem setor específico → `estratégia`

### 5. `agente` (sempre derivado, nunca manual)
- `nivel: estratégico` + `departamento: business` → `ceo`
- `nivel: tático` + `departamento: marketing` → `cmo` · `branding` → `cbo` · `produto` → `cpo` · `vendas` → `cso` · `finanças` → `cfo`
- `nivel: operacional` + `setor: S` → slug do setor (`copywriting`, `seo`, `precificação`, `naming`, …)
- `nivel: operacional` + `departamento: business` (sem setor) → `ceo` (ex.: BSC, VRiO)

### 6. Limpeza de `tags` (excluir de `tags` e, quando aplicável, mover para `author`)
1. Nomes de autores saem de `tags` → vão para `author` (ex.: Russell Brunson, Gabriel Weinberg, Justin Mares, Robert W. Bly, Ray Edwards, Georg Tacke, Madhavan Ramanujam, Margaret Mark, Carol S. Pearson, Marty Neumeier, Gary Halbert, Sinem Günel, Jenni Romaniuk, David A. Aaker, Byron Sharp, Robert Bartlett, Alexander Osterwalder, Douglas B. Holt, Donald Miller, Dan Harmon, Kevin L. Keller, Michael Porter, W. Chan Kim, Renée Mauborgne, Philip Kotler, Martin Lindstrom, Hudson Rennie).
2. Remover chaves vazadas como itens de tag: `author:`, `dominio:`, `Subdominio:`, `Sub_subdominio:`, `tipo:`, `up:`, `same:`, `down:`, `source:`, `description:`.
3. Remover hashtags textuais: `#funding`, `#case`.
4. Padronizar: minúsculas, acentos corretos, hífens (`marketing-tatico-mix` → `marketing-tático-mix`, `analise-mercados` → `análise-de-mercado`, `e-mail_marketing` → `e-mail-marketing`), remover duplicatas.
5. Manter tags temáticas livres (ex.: `persuasão`, `storytelling`, `realestate`, `funding`, `luxo`, `manifesto`, `design`).

### 7. Preservar
- `author` já existente (e autores recuperados das tags).
- `source` (URL) já existente.
- `description` já existente.
- Campos Breadcrumbs já existentes: `up`, `same`, `down`.

## Requisitos técnicos

- **Encoding UTF-8** na leitura e escrita (obrigatório para preservar acentos do PT-BR).
- Parsear apenas o bloco entre `---` e `---` no topo; todo o restante do arquivo deve permanecer **byte a byte intacto**.
- Gerar o frontmatter novo com indentação de 2 espaços em listas.
- Ordem dos campos conforme o bloco exemplo acima (consistência entre arquivos).
- Se um arquivo não tiver frontmatter, **crie** o bloco no topo.

## Entrega

1. Aplique em todos os 515 arquivos.
2. Relatório final com:
   - Contagem por `tipo`, `nivel`, `agente`, `departamento` (distribuição geral);
   - Lista de arquivos em que a classificação foi ambígua (precisou de julgamento) — não mais que ~20, com o valor escolhido;
   - Amostra de 5 exemplos por `tipo` (livro, capítulo, método, MOC, fonte, exemplo) com o frontmatter antes/depois;
   - `git diff --stat` do repo.
3. Crie a nota `3. Recursos\Estudos\Taxonomia de Metadata.md` com:
   - Organograma de agentes (tabela: agente · nível · escopo RAG · comanda · respondido por) e a cadeia de comando CEO → CMO/CBO/CPO/CSO/CFO → setores;
   - Definição de cada campo e os vocabulários controlados;
   - Templates de query de roteamento por agente;
   - Tabela de mapeamento antigo → novo e checklist para arquivos futuros.

## Restrições finais

- NÃO commitar, NÃO push, NÃO criar branches.
- NÃO modificar arquivos fora do escopo (Livros + Métodos) nem a nota `Taxonomia de Metadata.md` além do necessário.
- Em caso de dúvida de classificação, use bom senso pelo contexto do conteúdo; documente a decisão no relatório.
