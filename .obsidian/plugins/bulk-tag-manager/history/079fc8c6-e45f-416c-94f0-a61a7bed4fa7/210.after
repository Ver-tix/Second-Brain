---
tags:
  - IA
---

O ZCode **já suporta a criação de subagentes personalizados pela interface**, sem precisar editar arquivos manualmente. Quando você salva um subagente, ele é gravado como um arquivo Markdown em `~/.zcode/agents/` e pode ser chamado pelo nome nas conversas. ([ZCode](https://zcode.z.ai/en/docs/subagents?utm_source=chatgpt.com "Subagents | ZCODE Docs"))

# Passo a passo — Criando um agente no ZCode

| Passo  | O que fazer                                                                                                                                           |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**  | Abra o **ZCode**.                                                                                                                                     |
| **2**  | Vá em **Settings (⚙️)**.                                                                                                                              |
| **3**  | Abra a seção **Subagents**.                                                                                                                           |
| **4**  | Clique em `+` **New Subagent**.                                                                                                                       |
| **5**  | Escolha um nome para o agente.                                                                                                                        |
| **6**  | Escreva a descrição.                                                                                                                                  |
| **7**  | Defina as ferramentas (Tools) que ele poderá utilizar.                                                                                                |
| **8**  | Escreva o **System Prompt** do agente.                                                                                                                |
| **9**  | Salve. O ZCode criará automaticamente um arquivo em `~/.zcode/agents/`.                                                                               |
| **10** | Chame o agente na conversa usando `@nome-do-agente`. ([ZCode](https://zcode.z.ai/en/docs/subagents?utm_source=chatgpt.com "Subagents \| ZCODE Docs")) |

---

# Exemplo

## Nome

```text
business-architect
```

---

## Descrição

```text
Especialista em criação, análise e validação de modelos de negócios.
```

---

## Ferramentas

Você escolhe quais o agente poderá usar.

Exemplo:

|Ferramenta|Permitir?|
|---|---|
|Read|✅|
|Grep|✅|
|Glob|✅|
|Edit|❌|
|Write|❌|
|Bash|❌|

Esse agente seria apenas consultivo.

---

# System Prompt

Aqui está a inteligência do agente.

Exemplo:

```text
Você é um Business Architect.

Sua missão é ajudar empreendedores a desenhar negócios robustos.

Você domina:

- Business Model Canvas
- Lean Startup
- Estratégia
- Marketing
- Finanças
- Growth
- Valuation

Sempre responda utilizando:

1. Problema
2. Cliente
3. Mercado
4. Proposta de Valor
5. Modelo de Receita
6. Estrutura de Custos
7. Canais
8. Riscos
9. Próximos Passos

Nunca aceite premissas sem questioná-las.

Faça perguntas antes de propor soluções.
```

---

# Como usar

Depois de salvo:

```text
@business-architect

Quero criar uma startup para o mercado imobiliário.
```

Ou:

```text
@business-architect

Analise este Business Model Canvas.
```

O ZCode carregará automaticamente o System Prompt desse agente. ([ZCode](https://zcode.z.ai/en/docs/subagents?utm_source=chatgpt.com "Subagents | ZCODE Docs"))

---

# Onde ele fica salvo?

Depois de clicar em **Save**, o ZCode cria um arquivo como:

```text
~/.zcode/
│
└── agents/
    ├── business-architect.md
    ├── marketing.md
    ├── python-professor.md
    └── security-reviewer.md
```

Cada agente é um arquivo Markdown independente. ([ZCode](https://zcode.z.ai/en/docs/subagents?utm_source=chatgpt.com "Subagents | ZCODE Docs"))

---

# Um ecossistema que eu montaria

|Agente|Especialidade|
|---|---|
|`business-architect`|Modelagem de negócios|
|`venture-capitalist`|Avaliação de startups|
|`marketing-strategist`|Posicionamento e Growth|
|`brand-strategist`|Branding|
|`financial-analyst`|Fluxo de caixa, valuation e métricas|
|`python-professor`|Ensino de Python|
|`software-architect`|Arquitetura de software|
|`code-reviewer`|Revisão técnica|
|`security-reviewer`|Segurança|
|`technical-writer`|Documentação|

---

## Um insight para o seu caso

Pelo que conheço dos seus interesses ([[🔥Projeto Prometheus]], [[3. Inteligência Artificial HUB|Inteligência Artificial]], [[2.1. Marketing HUB|Marketing]], [[4. Mercado Imobiliário HUB|Imobiliário]] e [[1. Business HUB|business]]), eu iria um passo além: em vez de criar agentes por profissão ("marketing", "finanças"), criaria agentes por **papel no processo de construção de um negócio**.

Por exemplo:

|Agente|Responsabilidade|
|---|---|
|`opportunity-finder`|Descobre oportunidades de mercado.|
|`business-designer`|Desenha o modelo de negócio.|
|`market-validator`|Testa hipóteses e valida demanda.|
|`growth-planner`|Planeja aquisição e retenção de clientes.|
|`financial-modeler`|Constrói projeções financeiras e unit economics.|
|`execution-coach`|Transforma a estratégia em um plano de execução.|

Esse conjunto funciona como um **conselho consultivo virtual**: cada agente é especialista em uma etapa da criação de empresas, em vez de apenas representar uma área de conhecimento. Isso tende a produzir análises mais profundas e organizadas para projetos de empreendedorismo.