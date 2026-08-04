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

## Conteúdo do de `system_prompt.py`:
```Python
SYSTEM_PROMPT = """
Você faz parte do Prometheus OS.

Responda sempre em português.

Se não souber a resposta, diga que não sabe.

Nunca invente informações.
"""
```

# ==Etapa 2 - Refatorar o `mentor_prompt.p
O objetivo será deixá-lo contendo **somente a identidade do Prometheus Mentor**, removendo qualquer regra global que agora pertence ao `system_prompt.py`.==

Antes tínhamos:
```Python
class PromptBuilder:
  
    @staticmethod
    def build(history: str,question: str) -> str:

        prompt = f"""
Você é o Prometheus-Mentor. 

Sua missão é ensinar Inteligência Artificial e Engenharia de Software de forma clara, didática e organizada. 

Sempre explique os conceitos de maneira progressiva, começando pela intuição antes da definição técnica.

Histórico da conversa:

{history}

Nova pergunta do usuário: 

{question}
"""

        return prompt
```

Agora temos:
```python
MENTOR_PROMPT = """
Você é o Prometheus Mentor.

Seu objetivo é ensinar conceitos de Inteligência Artificial, Engenharia de Software e Arquitetura de Agentes.

Explique de forma didática.

Sempre que possível, utilize exemplos.

Explique passo a passo.

Adapte a profundidade da resposta ao contexto da conversa.
"""
```

## Então... para onde vai a classe `PromptBuilder`?
Ela sai deste arquivo completamente.

Você criará um novo arquivo (caso ainda não exista):
```
app/
└── prompts/
    ├── system_prompt.py
    ├── mentor_prompt.py
    └── prompt_builder.py   ← aqui ficará a classe
```

E a classe ficará parecida com isto:
```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT

class PromptBuilder:
    @staticmethod
    def build(history: str, question: str) -> str:

        return f"""
{SYSTEM_PROMPT}

{MENTOR_PROMPT}

Histórico da conversa:

{history}

Nova pergunta do usuário:

{question}
"""
```
Observe como agora cada componente tem uma única responsabilidade:

- `system_prompt.py` → regras globais.
- `mentor_prompt.py` → identidade do Mentor.
- `prompt_builder.py` → monta o prompt final.

Agora temos que fazer o seguinte: criar um arquivo `prompt_builder.py`, e pôr a classe `PromptBuilder` lá
```
app/
└── prompts/
    ├── system_prompt.py
    ├── mentor_prompt.py      # apenas MENTOR_PROMPT
    └── prompt_builder.py     # classe PromptBuilder
```

```Python
from app.prompts.system_prompt import SYSTEM_PROMPT
from app.prompts.mentor_prompt import MENTOR_PROMPT

class PromptBuilder:

    @staticmethod
    def build(history: str, question: str) -> str:
        return (
    f"{SYSTEM_PROMPT}\n\n"
    f"{MENTOR_PROMPT}\n\n"
    f"Histórico da conversa:\n"
    f"{history}\n\n"
    f"Nova pergunta do usuário:\n"
    f"{question}"
)
```

---
# Etapa 3