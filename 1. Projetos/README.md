---
tags:
  - instruction
  - marketing
  - business
  - inteligenciaartificial
  - RealEstate
---

# 📘 README — Como funciona esta pasta

> Orientação sobre a lógica de **1. Projetos** e sua relação com **2. Áreas** dentro do vault Second Brain (metodologia PARA).

---

## 1. A pilha de conhecimento em "2. Áreas"

As áreas formam uma **pilha de camadas**, onde cada uma se apoiou na anterior — dos fundamentos até o domínio de aplicação.

```
┌─────────────────────────────────────┐
│  4. Mercado Imobiliário             │  ← DOMÍNIO de aplicação (onde tudo converge)
├─────────────────────────────────────┤
│  3. Inteligência Artificial         │  ← MULTIPLICADOR (otimiza e automatiza)
├─────────────────────────────────────┤
│  2. Marketing                       │  ← COMO entregar e comunicar valor
├─────────────────────────────────────┤
│  1. Business                        │  ← O QUE é valor, estratégia, modelos
└─────────────────────────────────────┘
        fundamentos → aplicação
```

| Área                               | Pergunta que responde                                             | Natureza                                                                               |
| ---------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **[[1. Business HUB]]**                | "O que é valor? Como negócios criam e capturam valor?"            | **Fundamentos** — mental models (Porter, VRIO, BMC, Inovação)                          |
| **[[2. Marketing HUB]]**               | "Como transformar esse valor em ofertas que os clientes queiram?" | **Aplicação externa** — constrói sobre Business (posicionamos *valor*, não abstrações) |
| **[[3. Inteligência Artificial HUB]]** | "Como fazer mais, mais rápido, com menos esforço?"                | **Multiplicador transversal** — atravessa as duas anteriores                           |
| **[[4. Mercado Imobiliário HUB]]**     | "Onde, concretamente, vou aplicar tudo isso?"                     | **Domínio específico** — o ponto de convergência                                       |

### Observação de refinamento

As camadas **1 e 2 são sequenciais** de fato (Marketing precisa de Business antes). Mas a camada **3 (IA) é mais transversal do que sequencial** — ela não vem "depois" de Marketing, ela *amplifica* o que já existe nas duas anteriores. Vale pensá-la como uma **alavanca que cruza tudo**, não como um degrau obrigatório antes do Mercado Imobiliário.

---

## 2. O que é um Projeto (e como esta pasta se preenche)

A diferença entre **Área** e **Projeto** é o *prazo/fim*, não o tema:

> **Área** = competência que você mantém *para sempre*, sem data de fim.
> **Projeto** = esforço com **objetivo específico e data de conclusão**, que *combina* áreas para gerar um entregável.

Em outras palavras, **Projetos são fatias verticais que cortam as Áreas na horizontal**:

```
1.Business   2.Marketing   3.IA    4.Merc.Imob.
    │             │           │          │
    └─────────────┴───┬───────┴──────────┘   ← um PROJETO é esse corte transversal
                      │
                entregável concreto + prazo
```

---

## 3. Exemplos concretos (baseados neste vault)

| Projeto | Quais Áreas combina | Entregável |
|---------|--------------------|------------|
| **Incorporação Imobiliária** | Business + Marketing + IA + Mercado Imobiliário | Lançar um empreendimento / criar a operação — *o exemplo perfeito, toca nas 4 áreas* |
| **Projeto Prometheus** | principalmente IA | Concluir a formação em IA (tem fim: quando o curso termina) |
| *(futuro)* "Campanha de pré-lançamento do Empreendimento X" | Marketing + Mercado Imobiliário | Materiais prontos até uma data |
| *(futuro)* "Definir estratégia competitiva do meu negócio" | Business | Documento de estratégia até uma data |

### Duas observações importantes

1. **"Incorporação Imobiliária" é o protótipo do que deve morar em `1. Projetos`.** É o ponto onde *todas as quatro áreas convergem* num objetivo concreto.

2. **O mesmo tema pode aparecer nas duas pastas.** A **Área** guarda o *conhecimento* do domínio (ex.: Mercado Imobiliário); o **Projeto** guarda a *execução* de uma iniciativa específica (ex.: "lançamento do empreendimento Y até dezembro").

---

## 4. Estrutura sugerida para `1. Projetos/`

```
1. Projetos/
├── Incorporação Imobiliária/
│   ├── Visão e objetivos.md          ← combinação das 4 áreas
│   ├── Estratégia de negócio.md      ← puxa de Business
│   ├── Marketing e vendas.md         ← puxa de Marketing
│   ├── Automações com IA/            ← puxa de IA
│   └── (prompts e materiais relacionados)
│
└── Projeto Prometheus/
    └── (referência da formação em IA)
```

---

## 5. Ciclo de vida no PARA

Quando um projeto **termina**, ele (ou seus artefatos relevantes) migra para `4. Arquivos/` ou vira material de referência em `3. Recursos/`.

```
Projetos (ativos)  →  Arquivos (concluídos)
      ↑
   combinam Áreas para gerar um entregável com prazo
```

---

## Resumo

- **2. Áreas** = conhecimento permanente, em camadas (Business → Marketing → IA → Mercado Imobiliário).
- **1. Projetos** = execução com prazo, que combina Áreas para gerar um entregável concreto.
- Critério para criar um Projeto: **"tem objetivo e prazo, e combina Áreas?"** → se sim, mora aqui.
