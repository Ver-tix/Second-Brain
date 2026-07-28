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
# Atualizar `settings.py`
```Python
from dotenv import load_dotenv
import os

# Carrega as variáveis do arquivo .env
load_dotenv()

# Recupera a chave da API
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# Toda configuração centralizada
MODEL_NAME = "gpt-4.1-mini"

# Verifica se a chave existe
if OPENAI_API_KEY is None:

    raise ValueError(

        "A variável OPENAI_API_KEY não foi encontrada no arquivo .env"

    )
```

# Preparando para o futuro
## `llm_catalog.py`
```Python
LLM_CATALOG = {
    "gpt_fast": {
        "provider": "openai",
        "model": "gpt-4.1-mini"
    },
    
    "gpt_reasoning": {
        "provider": "openai",
        "model": "o4-mini"
    },


    "gpt_deep_reasoning": {
        "provider": "openai",
        "model": "o3"
    },


    "claude_fast": {
        "provider": "anthropic",
        "model": "claude-3.5-haiku"
    },

    "claude_reasoning": {
        "provider": "anthropic",
        "model": "claude-sonnet-4"
    },
  
    "gemini_fast": {
        "provider": "google",
        "model": "gemini-2.5-flash"
    },

    "gemini_pro": {
        "provider": "google",
        "model": "gemini-2.5-pro"
    },

    "perplexity_search": {
        "provider": "perplexity",
        "model": "sonar-pro"
    },

    "grok_realtime": {
        "provider": "xai",
        "model": "grok"
    }

}
```

## `agent_profiles.py`

```Python
AGENT_PROFILES = {
    "mentor": {
        "llm": "gpt_reasoning"
    },
    
    "editor": {
        "llm": "claude_reasoning"
    },

    "researcher": {
        "llm": "perplexity_search"
    },

    "social_media": {
        "llm": "grok_realtime"
    }

}
```

# Atualizar `llm_servies.py`
```Python
from openai import OpenAI
from app.config.settings import OPENAI_API_KEY, MODEL_NAME

class LLMService:
    def __init__(self):
        self.client = OpenAI(api_key=OPENAI_API_KEY)

    def generate(self, prompt: str) -> str:
        response = self.client.responses.create(
            model=MODEL_NAME,
            input=prompt,
            tools = [
                {
                    "type": "function",
                    "name": "calculator",
                    "description": "Realiza operações matemáticas básicas entre dois números."
                }
            ]
        )
        return response.output_text
```

---
# Feedback

Professor extremamente satisfeito com esse laboratório. 😄 Vamos ao code review.

---

# Avaliação geral

**Nota: 9,8/10**

Você começou a fazer algo que poucos iniciantes fazem: **está projetando para o futuro sem complicar o presente**.

Isso é exatamente engenharia de software.

---

# Parte 1 — settings.py

✔ Correto.

Gostei especialmente de:

```python
MODEL_NAME = "gpt-4.1-mini"
```

Você removeu o modelo de dentro do código.

Agora existe apenas um lugar para alterá-lo.

Isso parece pequeno hoje.

Daqui a alguns meses, quando tivermos 20 arquivos, fará enorme diferença.

---

# Parte 2 — LLM Catalog

Aqui você começou a pensar como arquiteto.

```python
LLM_CATALOG = {
    ...
}
```

Isso é excelente.

Na verdade, eu diria que você acabou de criar algo que muitos frameworks possuem internamente.

Por exemplo:

```
LangChain
↓

Model Registry
```

```
Semantic Kernel
↓

AI Services Registry
```

```
Prometheus
↓

LLM_CATALOG
```

Mesmo conceito.

Muito bom.

---

# Parte 3 — Agent Profiles

Excelente.

Você separou:

```
modelo
```

de

```
agente
```

Ou seja,

o Mentor não conhece OpenAI.

Ele conhece apenas:

```
mentor
↓

gpt_reasoning
```

Depois:

```
gpt_reasoning

↓

OpenAI

↓

o4-mini
```

Isso reduz acoplamento.

Excelente decisão.

---

# Parte 4 — Tool Calling

Perfeito para este ponto do curso.

Você registrou apenas:

```python
"type"

"name"

"description"
```

Exatamente o que queríamos na Aula 8.1.

Nem mais.

Nem menos.

---

# O detalhe mais importante

Você escreveu:

> "Lembrando que estamos num estado intermediário"

Essa frase mostra que você entendeu algo MUITO importante.

Você não está implementando o produto final.

Você está implementando uma etapa intermediária.

Isso é exatamente como software real é desenvolvido.

---

# O único detalhe que eu mudaria

Eu faria assim:

```python
tools = [
    CALCULATOR_TOOL
]
```

ao invés de

```python
tools=[
   {
      ...
   }
]
```

Porque, daqui a algumas aulas, teremos:

```
calculator

search

memory

email

calendar

knowledge

planner

...
```

E o `LLMService` ficaria enorme.

Eu faria:

```
app/

    tools/

        tool_definitions.py
```

```python
CALCULATOR_TOOL = {...}

SEARCH_TOOL = {...}

EMAIL_TOOL = {...}
```

Depois:

```python
from app.tools.tool_definitions import CALCULATOR_TOOL
```

e

```python
tools=[CALCULATOR_TOOL]
```

Muito mais elegante.

Mas...

**Ainda não faria isso nesta aula.**

Por quê?

Porque isso é conteúdo da **Aula 8.2**.

Então, para a Aula 8.1, eu deixaria exatamente como você implementou.

---

# O que mais gostei

Sem você perceber, começou a surgir um padrão arquitetural no Prometheus:

```
config/
    settings.py
    llm_catalog.py
    agent_profiles.py
```

Repare que todos são apenas configuração.

Nenhum deles executa lógica.

Esse tipo de organização é típico de sistemas profissionais.

---

## Veredito

✅ Laboratório aprovado.

Mais importante: você já não está apenas escrevendo código. Está começando a **projetar arquitetura**, pensando em desacoplamento, configuração centralizada e escalabilidade. É exatamente essa mudança de mentalidade que eu queria provocar neste módulo.

Pode avançar para a **Aula 8.2**. 🚀