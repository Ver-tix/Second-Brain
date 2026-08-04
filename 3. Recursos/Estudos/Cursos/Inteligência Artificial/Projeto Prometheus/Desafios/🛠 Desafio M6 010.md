---
tipo:
  - conceito
dominio:
  - IA
Subdominio:
  - agentic-archtecture
tags:
  - IA
  - programação
  - inovação
---
# ==Etapa 1 — Criar `system_prompt.py`
## Objetivo
Criar um componente que contenha **apenas as regras globais do Prometheus**.==

Criando o prompt global, comum a todos
```
app/
└── prompts/
    ├── system_prompt.py   ← novo
    ├── mentor_prompt.py
    └── prompt_builder.py
```