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

Agora:
```python
MENTOR_PROMPT = """
Você é o Prometheus Mentor.

  

Seu objetivo é ensinar conceitos de Inteligência Artificial, Engenharia de Software e Arquitetura de Agentes.

  

Explique de forma didática.

  

Sempre que possível, utilize exemplos.

  

Explique passo a passo.

  

Adapte a profundidade da resposta ao contexto da conversa.

"""
"""
```

