---
tags:
  - IA
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tipo:
  - sintese
---
# ARQUITETURA INTEGRADA PARA AGENTES DE IA FUNCIONAIS
```

                                  [ INTERFACE / USUÁRIO ]
                                             │
                                             ▼
                             ┌────────────────────────────────┐
                             │    1. ENTRADA & GOVERNANÇA     │
                             ├────────────────────────────────┤
                             │ • Input Guardrails             │
                             │ • Sanitização e Filtro PII     │
                             │ • Validação de Autenticação    │
                             └───────────────┬────────────────┘
                                             │
                                             ▼
                             ┌────────────────────────────────┐
                             │ 2. NÚCLEO DE COGNIÇÃO E PLANO  │
                             ├────────────────────────────────┤
                             │ • Engine de Modelo (LLM/SLM)   │
                             │ • CoT / ReAct / Plan-Execute   │
                             │ • Constituição & System Prompt │
                             └───────┬────────────────┬───────┘
                                     │                │
            ┌────────────────────────┘                └────────────────────────┐
            ▼                                                                  ▼
┌───────────────────────┐                                          ┌───────────────────────┐
│ 3. SISTEMA DE MEMÓRIA │                                          │ 4. ORQUESTRAÇÃO E     │
├───────────────────────┤                                          │    FERRAMENTAS        │
│ • Memória de Curto    │                                          ├───────────────────────┤
│   Prazo (Buffer)      │                                          │ • Tool Registry &     │
│ • Memória de Longo    │                                          │   Schemas (JSON)      │
│   Prazo (Vector /     │                                          │ • Sandbox Environment │
│   Graph RAG)          │                                          │ • Loop de Auto-       │
│ • Sumarizador de      │                                          │   Correção & Retries  │
│   Contexto            │                                          └───────────┬───────────┘
└───────────────────────┘                                                      │
                                                                               ▼
                                                                   ┌───────────────────────┐
                                                                   │ 5. VALIDAÇÃO E        │
                                                                   │    SAÍDA              │
                                                                   ├───────────────────────┤
                                                                   │ • Output Guardrails   │
                                                                   │ • Checagem RBAC       │
                                                                   │ • Confirmação Humana  │
                                                                   │   (HITL)              │
                                                                   └───────────┬───────────┘
                                                                               │
                                                                               ▼
                                                                   [ RESPOSTA AO USUÁRIO ]
                                                                               │
                                                                               ▼
                                                                   ┌───────────────────────┐
                                                                   │ 6. OBSERVABILIDADE &  │
                                                                   │    EVALS (BACKGROUND) │
                                                                   ├───────────────────────┤
                                                                   │ • Trajectory Logging  │
                                                                   │ • Pass@1 / Token Cost │
                                                                   │ • LLM-as-a-Judge      │
                                                                   └───────────────────────┘

---

## DETALHAMENTO DOS COMPONENTES

1. ENTRADA & GOVERNANÇA: Input Guardrails, Sanitização PII e Autenticação.
2. NÚCLEO DE COGNIÇÃO: Engine LLM, CoT/ReAct e Constituição/System Prompt.
3. MEMÓRIA: Buffer de Curto Prazo, Vector/Graph RAG e Sumarizador.
4. ORQUESTRAÇÃO & FERRAMENTAS: Schemas JSON, Sandbox e Auto-Correção.
5. VALIDAÇÃO DE SAÍDA: Output Guardrails, RBAC e Human-in-the-Loop.
6. OBSERVABILIDADE & EVALS: Logs de Trajetória, Custos/Pass@1 e LLM-as-a-Judge.
```